# 查询语音生成任务状态

> 使用本接口，查询异步语音合成任务状态。

## OpenAPI

````yaml api-reference/speech/t2a-async/api/openapi.json get /v1/query/t2a_async_query_v2
paths:
  path: /v1/query/t2a_async_query_v2
  method: get
  servers:
    - url: https://api.minimaxi.com
  request:
    security:
      - title: bearerAuth
        parameters:
          query: {}
          header:
            Authorization:
              type: http
              scheme: bearer
              description: |-
                `HTTP: Bearer Auth`
                 - Security Scheme Type: http
                 - HTTP Authorization Scheme: Bearer API_key，用于验证账户信息，可在 [账户管理>接口密钥](https://platform.minimaxi.com/user-center/basic-information/interface-key) 中查看。
          cookie: {}
    parameters:
      path: {}
      query:
        task_id:
          schema:
            - type: integer
              required: true
              description: 任务 ID，提交任务时返回的信息
      header: {}
      cookie: {}
    body: {}
  response:
    '200':
      application/json:
        schemaArray:
          - type: object
            properties:
              task_id:
                allOf:
                  - type: integer
                    format: int64
                    description: 任务 ID
              status:
                allOf:
                  - type: string
                    description: |-
                      该任务的当前状态。

                      - **Processing**: 该任务正在处理中
                      - **Success**: 该任务已完成
                      - **Failed**: 任务失败
                      - **Expired**: 任务已过期
                    enum:
                      - success
                      - failed
                      - expired
                      - processing
              file_id:
                allOf:
                  - type: integer
                    format: int64
                    description: >-
                      任务创建成功后返回的对应音频文件的 ID。

                      - 当任务完成后，可通过 file_id 调用
                      [文件检索接口](/docs/api-reference/file-management-retrieve)
                      进行下载

                      - 当请求出错时，不返回该字段

                      注意：返回的下载 URL 自生成起 9 小时（32,400
                      秒）内有效，过期后文件将失效，生成的信息便会丢失，请注意下载信息的时间
              base_resp:
                allOf:
                  - $ref: '#/components/schemas/BaseResp'
            refIdentifier: '#/components/schemas/T2AAsyncV2QueryResp'
            example:
              task_id: 95157322514444
              status: Processing
              file_id: 95157322514496
              base_resp:
                status_code: 0
                status_msg: success
        examples:
          example:
            value:
              task_id: 95157322514444
              status: Processing
              file_id: 95157322514496
              base_resp:
                status_code: 0
                status_msg: success
        description: ''
  deprecated: false
  type: path
components:
  schemas:
    BaseResp:
      type: object
      description: 本次请求的状态码及其详情
      required:
        - status_code
        - status_msg
      properties:
        status_code:
          type: integer
          format: int64
          description: |-
            状态码

            - `0`: 正常
            - `1002`: 限流
            - `1004`: 鉴权失败
            - `1039`: 触发 TPM 限流
            - `1042`: 非法字符超10%
            - `2013`: 参数错误

            更多内容可查看 [错误码查询列表](/docs/api-reference/errorcode) 了解详情
        status_msg:
          type: string
          description: 状态详情

````