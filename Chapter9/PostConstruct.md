# 静态工具类调用service

### 场景
静态工具类里调用service。需要注入。比如请求工具类里调用了日志service.

**代码 **

工具类MsRequestUtil

工具类需要注入bean工厂，日志类 CcMsRequestLogService 通过 @PostConstruct方法注入。

【常识】

**在spring项目中，在一个bean的初始化过程中，方法执行先后顺序为**

**Constructor > @Autowired > @PostConstruct**

**先执行完构造方法，再注入依赖，最后执行初始化操作**

如下MsRequestUtil类。若没有 

```
    @PostConstruct
    public void init() {
        instance = this;
    }
```

this此时已包含了变量CcMsRequestLogService（已注入的）。



则MsRequestUtil构造方法执行后，CcMsRequestLogService 才被注入（   @Resource）。

此时MsRequestUtil.getInstance().ccMsRequestLogService为空。然后CcMsRequestLogService 注入后，MsRequestUtil.getInstance().ccMsRequestLogService还是为空。



而

```
@Slf4j
@Component
public class MsRequestUtil {

    private volatile  static MsRequestUtil instance;

    private MsRequestUtil(){

    }

    public static  MsRequestUtil getInstance() {
        if (instance == null) {
            synchronized (MsRequestUtil.class) {
                if (instance == null) {
                    instance = new MsRequestUtil();
                }
            }
        }
        return instance;
    }

    @Resource
    CcMsRequestLogService ccMsRequestLogService;

    @PostConstruct
    public void init() {
        instance = this;
    }

    

    public static CpeRequestBasicDto getRequestBasicDto() {
        CpeRequestBasicDto cpeRequestBasicDto = new CpeRequestBasicDto();

        //定值
        cpeRequestBasicDto.setInstid(MsConstants.INSTID);
        cpeRequestBasicDto.setMchntid(MsConstants.MCHNTID);
        cpeRequestBasicDto.setTermid(MsConstants.TERMID);
        cpeRequestBasicDto.setCity(MsConstants.CITY);
        cpeRequestBasicDto.setTranstype(MsConstants.TRANSTYPE);
        cpeRequestBasicDto.setPayType(MsConstants.TRANSTYPE);

        Date now = new Date();
        String ymdStr = DateUtils.format(now, DateUtils.DATE_TIME_PATTERN2);
        String timeStr = DateUtils.format(now, DateUtils.TIME_PATTERN_HHMMSS);

        //请求流水号
        String syssesq = ymdStr + IdUtil.getId();
        cpeRequestBasicDto.setSyssesq(syssesq);
        //交易日期 yyyymmdd
        cpeRequestBasicDto.setTxndate(ymdStr);
        ///** 交易时间 Hh24mmss*/
        cpeRequestBasicDto.setTxntime(timeStr);


        return cpeRequestBasicDto;

    }

    /**
     * 获取最终请求dto
     *
     * @return
     */
    public static MsRequestDto getCpeRequestDto(MsRequestTxninfoDto msRequestTxninfoDto) {
        //请求小额系统 参数准备
        MsRequestDto msRequestDto = new MsRequestDto();
        //请求业务参数
        JSONObject jsonObject = (JSONObject) JSON.toJSON(msRequestTxninfoDto);
        String bizJsonStr = jsonObject.toJSONString();
        //set业务参数
        msRequestDto.setTxninfo(bizJsonStr);
        //签名
        String signcode = "";
        try {
            signcode = Sha1Utils.genSignHexByRSA(bizJsonStr, "utf-8", MsConstants.SELF_MS_PRIVATE_KEY).toUpperCase();

        } catch (Exception e) {
            e.printStackTrace();
            //获取签名异常
        }
        //set签名
        msRequestDto.setSigncode(signcode);
        return msRequestDto;

    }


    /**
     * 调用小额系统
     *
     * @param url
     * @param headers
     * @param msRequestDto 请求dto
     * @return R <MsResponseDto>
     */
    public static R doMsPost(String url, Map<String, String> headers, MsRequestDto msRequestDto) {
        R r = R.ok();
        //异常情况下 小额系统返回
        String resException="";
        try {

            String jsonStr = JSONObject.toJSONString(msRequestDto);

            log.info("*****************************************");
            String str = "data="+jsonStr;
            log.info("小额系统请求 {}" + str);
            //请求头 text/plain
            String res = OkHttpClientUtil.doPostText(url, headers, str);
            resException = res;
            log.info("小额系统返回 {}" + res);
            log.info("*****************************************");
            JSONObject msResJsonObject = JSONObject.parseObject(res);
            String  txninfoStr = msResJsonObject.get("txninfo").toString();

            String signcode = msResJsonObject.getString("signcode");
            //验签
            boolean signFlag = checkMsResSign(txninfoStr,signcode);
            if(!signFlag){
                return R.error(ErrorCodeCcBiz.MS_RES_SIGN_ERROR);
            }
            JSONObject txninfoObject = JSON.parseObject(txninfoStr);
            String responseCode = txninfoObject.getString(MsConstants.RESPONSE_CODE_STR);
            String responseDesc = txninfoObject.getString(MsConstants.RESPONSE_DESC_STR);
            if (MsConstants.RESPONSE_CODE_SUCCESS.equals(responseCode)) {
                r = R.ok();
            } else {
                r = R.error(responseCode +"(" + responseDesc + ")");
                //查询订单接口 订单已取消 算成功
                if(MsConstants.RESPONSE_CODE_QUERY_ORDER_CANCEL.equals(responseCode)
                        && MsConstants.TXNCODE_ACQ_ORDER_QUERY.equals(txninfoObject.getString("txncode"))){
                    r = R.ok();
                }
            }
           // MsResponseDto msResponseDto = JSON.parseObject(res, MsResponseDto.class);
            MsResponseDto msResponseDto = JSONObject.parseObject(txninfoStr,MsResponseDto.class);
            r.putData(msResponseDto);
            //调用小额系统日志 addMsLog
            MsRequestUtil.getInstance().ccMsRequestLogService.addMsLog(jsonStr, msRequestDto, res, msResponseDto);
            return r;
        } catch (Exception e) {
            log.info("MsRequestUtil->doMsPost e {}",e.toString());
            e.printStackTrace();
            //调用小额系统日志异常
            String exceptionMsg = "异常信息";
            if(e.toString().length()>800){
                exceptionMsg = e.toString().substring(0,800);
            }else{
                exceptionMsg = e.toString();
            }
            String  className = e.getStackTrace()[0].getClassName();
            String  mgetMethodName = e.getStackTrace()[0].getMethodName();
            int  lineNumber = e.getStackTrace()[0].getLineNumber();
            exceptionMsg = exceptionMsg +"||"+className+"->"+mgetMethodName+";lineNumber:"+lineNumber;
            MsRequestUtil.getInstance().ccMsRequestLogService.addMsLogException(msRequestDto,resException,exceptionMsg);
            log.info("调用小额系统接口异常 url {}, param {}, e {}", url, JSONObject.toJSONString(msRequestDto), e.toString(), e.toString());
            return R.error("调用小额系统接口异常 url" + url + "param" + JSONObject.toJSONString(msRequestDto));
        }
    }
}
```

