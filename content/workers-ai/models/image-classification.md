---
title: Image classification
pcx_content_type: get-started
weight: 5
---

# Image classification
ResNet models perform image classification - they take images as input and classify the major object in the image.

* ID:  **@cf/microsoft/resnet-50** - used to `run` this model via SDK or API
* Name: Resnet50 image classification model
* Task: image-classification
* License type: Apache 2.0
* [Terms + Information](https://huggingface.co/microsoft/resnet-50)

## Examples

{{<tabs labels="worker | curl">}}
{{<tab label="worker" default="true">}}

```ts
import { Ai } from '@cloudflare/ai'

export interface Env {
  AI: any;
}

export default {
  async fetch(request: Request, env: Env) {
    const res: any = await fetch("https://cataas.com/cat");
    const blob = await res.arrayBuffer();

    const ai = new Ai(env.AI);
    const inputs = {
        image: [...new Uint8Array(blob)],
    };

    const response = await ai.run("@cf/microsoft/resnet-50", inputs);

    return new Response(JSON.stringify({ inputs: { image: [] }, response }));
  }
}
```

{{</tab>}}

{{<tab label="curl">}}

```sh
$ curl https://api.cloudflare.com/client/v4/accounts/{account_id}/ai/run/@cf/microsoft/resnet-50 \
    -X POST \
    -H "Authorization: Bearer {API_TOKEN}" \
    --data-binary @orange-llama.png
```

{{</tab>}}
{{</tabs>}}

**Example Workers AI response**

```json
{
    "inputs": { "image":[] },
    "response":[
        {"label":"CARDIGAN","score":0.7912509441375732},
        {"label":"FRENCH BULLDOG","score":0.0382106676697731},
        {"label":"BOSTON BULL","score":0.0275872815400362},
        {"label":"PEMBROKE","score":0.01957731693983078},
        {"label":"GERMAN SHEPHERD","score":0.016647251322865486}
    ]
}

```

## API schema
The following schema is based on [JSON Schema](https://json-schema.org/)

```json
{
    "task": "image-classification",
    "tsClass": "AiImageClassification",
    "jsonSchema": {
        "input": {
            "type": "object",
            "properties": {
                "image": {
                    "type": "string",
                    "format": "binary"
                }
            },
            "required": ["image"]
        },
        "output": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "score": {
                        "type": "number"
                    },
                    "label": {
                        "type": "string"
                    }
                }
            }
        }
    }
}
```