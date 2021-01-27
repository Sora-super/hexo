---
title: http及其code码封装
date: 2021-01-25 17:53:45
comments: true
tags:
	- 常见问题
---





code|描述|使用函数
--|:--:|--
200	|成功	|.success(data)
400	|参数校验失败	|.validateFailed(param)
404	|接口不存在|
500	|请求失败|.fail(errdata)
700	|操作过于频繁|





#### 使用示例

```javascript

const ReturnResult = require('./result')

// 成功调用
ctx.body = ReturnResult.success(data)

// 参数验证失败调用
ctx.body = ReturnResult.validateFailed()

// 请求失败调用
ctx.body = ReturnResult.fail()
```





以下为具体封装代码


result.js
``` javascript
/**
 * @author xzx
 * @description 封装返回
 *
 */
const ResultCode = require('./resultCode')


class Result{
    constructor(code, message, data){
        this.code = code
        this.message = message
        this.data = data
    }

    /**
     * 成功
     * @param data {any} 返回对象
     * @return {Result}
     */
    static success(data) {
        return new Result(ResultCode.SUCCESS.code, ResultCode.SUCCESS.desc, data)
    }

    /**
     * 失败
     */
    static fail(errData) {
        return new Result(ResultCode.FAILED.code, ResultCode.FAILED.desc, errData)
    }

    /**
     * 参数校验失败
     */
    static validateFailed(param) {
        return new Result(ResultCode.VALIDATE_FAILED.code, ResultCode.VALIDATE_FAILED.desc, param)
    }

}

module.exports = Result

```

resultCode.js
``` javascript
/**
 * @author xzx
 * @description 业务异常通用code
 *
 */
class ResultCode {
    /**
     * code 状态码
     * desc 描述
    */
    constructor(code, desc) {
        this.code = code
        this.desc = desc
    }

    static SUCCESS = new ResultCode(200, '成功')
    static FAILED = new ResultCode(500, '失败')
    static VALIDATE_FAILED = new ResultCode(400, '参数校验失败')
    static API_NOT_FOUNT = new ResultCode(404, '接口不存在')
    static API_BUSY = new ResultCode(700, '操作过于频繁')
}

module.exports = ResultCode
```


