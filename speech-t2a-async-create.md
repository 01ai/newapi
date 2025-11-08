# 创建异步语音合成任务

> 使用本接口，创建异步语音合成任务。

## OpenAPI

````yaml api-reference/speech/t2a-async/api/openapi.json post /v1/t2a_async_v2
paths:
  path: /v1/t2a_async_v2
  method: post
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
      query: {}
      header:
        Content-Type:
          schema:
            - type: enum<string>
              enum:
                - application/json
              required: true
              description: 请求体的媒介类型，请设置为 `application/json`，确保请求数据的格式为 JSON
              default: application/json
      cookie: {}
    body:
      application/json:
        schemaArray:
          - type: object
            properties:
              model:
                allOf:
                  - type: string
                    description: >-
                      请求的模型版本，可选范围：`speech-2.6-hd`, `speech-2.6-turbo`,
                      `speech-02-hd`, `speech-02-turbo`, `speech-01-hd`,
                      `speech-01-turbo`.
                    enum:
                      - speech-2.6-hd
                      - speech-2.6-turbo
                      - speech-02-hd
                      - speech-02-turbo
                      - speech-01-hd
                      - speech-01-turbo
              text:
                allOf:
                  - type: string
                    description: 待合成音频的文本，限制最长 5 万字符。和 `text_file_id` 二选一必填
              text_file_id:
                allOf:
                  - type: integer
                    format: int64
                    description: "待合成音频的文本文件 待合成音频的文本文件 id，单个文件长度限制小于 10 万字符，支持的文件格式：txt、zip。和 `text` 二选一必填，传入后自动校验格式。\n- **txt 文件**：长度限制 <100,000 字符。支持使用 <#x#> 标记自定义停顿。x 为停顿时长（单位：秒），范围 [0.01,99.99]，最多保留两位小数。注意停顿需设置在两个可以语音发音的文本之间，不可连续使用多个停顿标记\n- **zip 文件**：\n\t- 压缩包内需包含同一格式的 txt 或 json 文件。\n\t- json 文件格式：支持 [`title`, `content`, `extra`] 三个字段，分别表示标题、正文、附加信息。若三个字段都存在，则产出 3 组结果，共 9 个文件，统一存放在一个文件夹中。若某字段不存在或内容为空，则该字段不会生成对应结果"
              voice_setting:
                allOf:
                  - $ref: '#/components/schemas/T2AAsyncV2VoiceSetting'
              audio_setting:
                allOf:
                  - $ref: '#/components/schemas/T2AAsyncV2AudioSetting'
              pronunciation_dict:
                allOf:
                  - $ref: '#/components/schemas/T2AAsyncV2PronunciationDict'
              language_boost:
                allOf:
                  - type: string
                    description: 是否增强对指定的小语种和方言的识别能力。默认值为 `null`，可设置为 `auto` 让模型自主判断。
                    enum:
                      - Chinese
                      - Chinese,Yue
                      - English
                      - Arabic
                      - Russian
                      - Spanish
                      - French
                      - Portuguese
                      - German
                      - Turkish
                      - Dutch
                      - Ukrainian
                      - Vietnamese
                      - Indonesian
                      - Japanese
                      - Italian
                      - Korean
                      - Thai
                      - Polish
                      - Romanian
                      - Greek
                      - Czech
                      - Finnish
                      - Hindi
                      - Bulgarian
                      - Danish
                      - Hebrew
                      - Malay
                      - Persian
                      - Slovak
                      - Swedish
                      - Croatian
                      - Filipino
                      - Hungarian
                      - Norwegian
                      - Slovenian
                      - Catalan
                      - Nynorsk
                      - Tamil
                      - Afrikaans
                      - auto
                    default: null
              voice_modify:
                allOf:
                  - $ref: '#/components/schemas/VoiceModify'
              aigc_watermark:
                allOf:
                  - type: boolean
                    description: 控制在合成音频的末尾添加音频节奏标识，默认值为 False。该参数仅对非流式合成生效
                    default: false
            required: true
            refIdentifier: '#/components/schemas/T2AAsyncV2Req'
            requiredProperties:
              - model
              - text
              - text_file_id
              - voice_setting
        examples:
          文本输入:
            value:
              model: speech-2.6-hd
              text: 真正的危险不是计算机开始像人一样思考，而是人开始像计算机一样思考。计算机只是可以帮我们处理一些简单事务。
              language_boost: auto
              voice_setting:
                voice_id: audiobook_male_1
                speed: 1
                vol: 1
                pitch: 1
              pronunciation_dict:
                tone:
                  - 危险/dangerous
              audio_setting:
                audio_sample_rate: 32000
                bitrate: 128000
                format: mp3
                channel: 2
              voice_modify:
                pitch: 0
                intensity: 0
                timbre: 0
                sound_effects: spacious_echo
          文件输入:
            value:
              model: speech-2.6-hd
              text_file_id: text_file_id
              language_boost: auto
              voice_setting:
                voice_id: audiobook_male_1
                speed: 1
                vol: 10
                pitch: 1
              pronunciation_dict:
                tone:
                  - 草地/(cao3)(di1)
              audio_setting:
                audio_sample_rate: 32000
                bitrate: 128000
                format: mp3
                channel: 2
              voice_modify:
                pitch: 0
                intensity: 0
                timbre: 0
                sound_effects: spacious_echo
  response:
    '200':
      application/json:
        schemaArray:
          - type: object
            properties:
              task_id:
                allOf:
                  - type: string
                    description: 当前任务的 ID
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
              task_token:
                allOf:
                  - type: string
                    description: 完成当前任务使用的密钥信息
              usage_characters:
                allOf:
                  - type: integer
                    description: 计费字符数
              base_resp:
                allOf:
                  - $ref: '#/components/schemas/BaseResp'
            refIdentifier: '#/components/schemas/T2AAsyncV2Resp'
        examples:
          文本输入:
            value:
              task_id: 95157322514444
              task_token: eyJhbGciOiJSUz
              file_id: 95157322514444
              usage_characters: 101
              base_resp:
                status_code: 0
                status_msg: success
          文件输入:
            value:
              task_id: 95157322514444
              task_token: eyJhbGciOiJSUz
              file_id: 95157322514444
              usage_characters: 101
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
    T2AAsyncV2VoiceSetting:
      type: object
      required:
        - voice_id
      properties:
        voice_id:
          type: string
          description: "合成音频的音色编号。若需要设置混合音色，请设置 timbre_weights 参数，本参数设置为空值。支持系统音色、复刻音色以及文生音色三种类型，以下是部分最新的系统音色（ID），可查看 [系统音色列表](/docs/faq/system-voice-id) 或使用 [查询可用音色 API](/docs/api-reference/speech-t2a-async-query) 查询系统支持的全部音色\n\n - **中文**:\n\t- moss_audio_ce44fc67-7ce3-11f0-8de5-96e35d26fb85\n\t- moss_audio_aaa1346a-7ce7-11f0-8e61-2e6e3c7ee85d\n\t- Chinese (Mandarin)_Lyrical_Voice\n\t- Chinese (Mandarin)_HK_Flight_Attendant\n- **英文**:\n\t- English_Graceful_Lady\n\t- English_Insightful_Speaker\n\t- English_radiant_girl\n\t- English_Persuasive_Man\n\t- moss_audio_6dc281eb-713c-11f0-a447-9613c873494c\n\t- moss_audio_570551b1-735c-11f0-b236-0adeeecad052\n\t- moss_audio_ad5baf92-735f-11f0-8263-fe5a2fe98ec8\n\t- English_Lucky_Robot\n- **日文**:\n\t- Japanese_Whisper_Belle\n\t- moss_audio_24875c4a-7be4-11f0-9359-4e72c55db738\n\t- moss_audio_7f4ee608-78ea-11f0-bb73-1e2a4cfcd245\n\t- moss_audio_c1a6a3ac-7be6-11f0-8e8e-36b92fbb4f95"
        speed:
          type: number
          format: float
          description: 合成音频的语速，取值越大，语速越快。取值范围 `[0.5,2]`，默认值为1.0
          minimum: 0.5
          maximum: 2
          default: 1
        vol:
          type: number
          format: float
          description: 合成音频的音量，取值越大，音量越高。取值范围 `(0,10]`，默认值为 1.0
          exclusiveMinimum: 0
          maximum: 10
          default: 1
        pitch:
          type: integer
          description: 合成音频的语调，取值范围 `[-12,12]`，默认值为 0，其中 0 为原音色输出
          minimum: -12
          maximum: 12
          default: 0
        emotion:
          type: string
          description: "控制合成语音的情绪，参数范围 `[\"happy\", \"sad\", \"angry\", \"fearful\", \"disgusted\", \"surprised\", \"calm\", \"fluent\"]`，分别对应 8 种情绪：高兴，悲伤，愤怒，害怕，厌恶，惊讶，中性，生动 \r\n- 模型会根据输入文本自动匹配合适的情绪，一般无需手动指定  \r\n- 该参数仅对 `speech-2.6-hd`, `speech-2.6-turbo`, `speech-02-hd`, `speech-02-turbo`, `speech-01-hd`, `speech-01-turbo` 模型生效 "
          enum:
            - happy
            - sad
            - angry
            - fearful
            - disgusted
            - surprised
            - calm
            - fluent
        english_normalization:
          type: boolean
          description: 支持英语文本规范化，开启后可提升数字阅读场景的性能，但会略微增加延迟，默认 false
          default: false
    T2AAsyncV2AudioSetting:
      type: object
      properties:
        audio_sample_rate:
          type: integer
          format: int64
          default: 32000
          description: 生成音频的采样率。可选范围 `[8000，16000，22050，24000，32000，44100]`，默认为 `32000`
        bitrate:
          type: integer
          format: int64
          default: 128000
          description: >-
            生成音频的比特率。可选范围 `[32000，64000，128000，256000]`，默认值为 `128000`。该参数仅对
            `mp3` 格式的音频生效
        format:
          type: string
          description: 生成音频的格式。可选范围`[mp3, pcm, flac]`，默认值为 `mp3`
          enum:
            - mp3
            - pcm
            - flac
          default: mp3
        channel:
          type: integer
          format: int64
          default: 2
          description: 生成音频的声道数。可选范围：`[1,2]`，其中 `1` 为单声道，`2` 为双声道，默认值为 1
    T2AAsyncV2PronunciationDict:
      type: object
      properties:
        tone:
          type: array
          description: |-
            定义需要特殊标注的文字或符号对应的注音或发音替换规则。在中文文本中，声调用数字表示：
            一声为 1，二声为 2，三声为 3，四声为 4，轻声为 5
            示例如下：
            `["燕少飞/(yan4)(shao3)(fei1)", "omg/oh my god"]`
          items:
            type: string
    VoiceModify:
      type: object
      description: 声音效果器设置
      properties:
        pitch:
          type: integer
          description: >-
            音高调整（低沉/明亮），范围 [-100,100]，数值接近 -100，声音更低沉；接近 100，声音更明亮


            ![pitch
            adjustment](https://filecdn.minimax.chat/public/5d210c47-4236-4e81-893b-16cc1ef0302d.png)
          minimum: -100
          maximum: 100
        intensity:
          type: integer
          description: >-
            强度调整（力量感/柔和），范围 [-100,100]，数值接近 -100，声音更刚劲；接近 100，声音更轻柔


            ![intensity
            adjustment](https://filecdn.minimax.chat/public/862d493e-71d5-4d1f-b7c3-9ac51890631b.png)
          minimum: -100
          maximum: 100
        timbre:
          type: integer
          description: >-
            音色调整（磁性/清脆），范围 [-100,100]，数值接近 -100，声音更浑厚；数值接近 100，声音更清脆


            ![timbre
            adjustment](https://filecdn.minimax.chat/public/5f0e6cae-363a-452b-8d42-fbc4ef5a0510.png)
          minimum: -100
          maximum: 100
        sound_effects:
          type: string
          description: |-
            音效设置，单次仅能选择一种，可选值：
            1. spacious_echo（空旷回音）
            2. auditorium_echo（礼堂广播）
            3. lofi_telephone（电话失真）
            4. robotic（电音）
          enum:
            - spacious_echo
            - auditorium_echo
            - lofi_telephone
            - robotic

````