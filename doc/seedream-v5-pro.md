# seedream-v5-pro

## OpenAPI Specification

```yaml
openapi: 3.0.1
info:
  title: ''
  description: ''
  version: 1.0.0
paths:
  /v1/images/generations:
    post:
      summary: seedream-v5-pro
      deprecated: false
      description: ''
      tags:
        - 绘图模型/OpenAI Dall-e 格式
      parameters:
        - name: Authorization
          in: header
          description: ''
          required: false
          example: Bearer {{YOUR_API_KEY}}
          schema:
            type: string
            default: Bearer {{YOUR_API_KEY}}
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                model:
                  type: string
                  x-apifox-mock: seedream-v5-pro
                prompt:
                  type: string
                size:
                  type: string
                  description: 1024~2048 1k 2k
                image:
                  type: array
                  items:
                    type: string
                response_format:
                  type: string
                  description: '"url" or "b64_json". 默认 b64_json'
                output_format:
                  type: string
                  description: png\jpg
              required:
                - model
                - prompt
              x-apifox-orders:
                - model
                - prompt
                - size
                - response_format
                - output_format
                - image
            examples: {}
      responses:
        '200':
          description: ''
          content:
            application/json:
              schema:
                type: object
                properties: {}
                x-apifox-orders: []
          headers: {}
          x-apifox-name: 成功
      security: []
      x-apifox-folder: 绘图模型/OpenAI Dall-e 格式
      x-apifox-status: released
      x-run-in-apifox: https://app.apifox.com/web/project/3868318/apis/api-485030908-run
components:
  schemas: {}
  securitySchemes: {}
servers: []
security: []

```