---
title: "Blog 1"
date: "2025-09-10"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

Getting started
The first thing is to make sure that you have access to the Nova Canvas model through the usual means. Head to the Amazon Bedrock console, choose Model access and enable Amazon Nova Canvas for your account making sure that you select the appropriate regions for your workloads. If you already have access and have been using Nova Canvas, you can start using the new features immediately as they’re automatically available to you.

Virtual try-on
The first exciting new feature is virtual try-on. With this, you can upload two pictures and ask Amazon Nova Canvas to put them together with realistic results. These could be pictures of apparel, accessories, home furnishings, and any other products including clothing. For example, you can provide the picture of a human as the source image and the picture of a garment as the reference image, and Amazon Nova Canvas will create a new image with that same person wearing the garment. Let’s try this out!

My starting point is to select two images. I picked one of myself in a pose that I think would work well for a clothes swap and a picture of an AWS-branded hoodie.

Matheus and AWS-branded hoodie

Note that Nova Canvas accepts images containing a maximum of 4.1M pixels – the equivalent of 2,048 x 2,048 – so be sure to scale your images to fit these constraints if necessary. Also, if you’d like to run the Python code featured in this article, ensure you have Python 3.9 or later installed as well as the Python packages boto3 and pillow.

To apply the hoodie to my photo, I use the Amazon Bedrock Runtime invoke API. You can find full details on the request and response structures for this API in the Amazon Nova User Guide. The code is straightforward, requiring only a few inference parameters. I use the new taskType of "VIRTUAL_TRY_ON". I then specify the desired settings, including both the source image and reference image, using the virtualTryOnParams object to set a few required parameters. Note that both images must be converted to Base64 strings.

import base64


def load_image_as_base64(image_path): 
   """Helper function for preparing image data."""
   with open(image_path, "rb") as image_file:
      return base64.b64encode(image_file.read()).decode("utf-8")


inference_params = {
   "taskType": "VIRTUAL_TRY_ON",
   "virtualTryOnParams": {
      "sourceImage": load_image_as_base64("person.png"),
      "referenceImage": load_image_as_base64("aws-hoodie.jpg"),
      "maskType": "GARMENT",
      "garmentBasedMask": {"garmentClass": "UPPER_BODY"}
   }
}
Python
Nova Canvas uses masking to manipulate images. This is a technique that allows AI image generation to focus on specific areas or regions of an image while preserving others, similar to using painter’s tape to protect areas you don’t want to paint.

You can use three different masking modes, which you can choose by setting maskType to the correct value. In this case, I’m using "GARMENT", which requires me to specify which part of the body I want to be masked. I’m using "UPPER_BODY" , but you can use others such as "LOWER_BODY", "FULL_BODY", or "FOOTWEAR" if you want to specifically target the feet. Refer to the documentation for a full list of options.

I then call the invoke API, passing in these inference arguments and saving the generated image to disk.

# Note: The inference_params variable from above is referenced below.

import base64
import io
import json

import boto3
from PIL import Image

# Create the Bedrock Runtime client.
bedrock = boto3.client(service_name="bedrock-runtime", region_name="us-east-1")

# Prepare the invocation payload.
body_json = json.dumps(inference_params, indent=2)

# Invoke Nova Canvas.
response = bedrock.invoke_model(
   body=body_json,
   modelId="amazon.nova-canvas-v1:0",
   accept="application/json",
   contentType="application/json"
)

# Extract the images from the response.
response_body_json = json.loads(response.get("body").read())
images = response_body_json.get("images", [])

# Check for errors.
if response_body_json.get("error"):
   print(response_body_json.get("error"))

# Decode each image from Base64 and save as a PNG file.
for index, image_base64 in enumerate(images):
   image_bytes = base64.b64decode(image_base64)
   image_buffer = io.BytesIO(image_bytes)
   image = Image.open(image_buffer)
   image.save(f"image_{index}.png")
Python
I get a very exciting result!

Matheus wearing AWS-branded hoodie

And just like that, I’m the proud wearer of an AWS-branded hoodie!

In addition to the "GARMENT" mask type, you can also use the "PROMPT" or "IMAGE" masks. With "PROMPT", you also provide the source and reference images, however, you provide a natural language prompt to specify which part of the source image you’d like to be replaced. This is similar to how the "INPAINTING" and "OUTPAINTING" tasks work in Nova Canvas. If you want to use your own image mask, then you choose the "IMAGE" mask type and provide a black-and-white image to be used as mask, where black indicates the pixels that you want to be replaced on the source image, and white the ones you want to preserve.

This capability is specifically useful for retailers. They can use it to help their customers make better purchasing decisions by seeing how products look before buying.

Using style options
I’ve always wondered what I would look like as an anime superhero. Previously, I could use Nova Canvas to manipulate an image of myself, but I would have to rely on my good prompt engineering skills to get it right. Now, Nova Canvas comes with pre-trained styles that you can apply to your images to get high-quality results that follow the artistic style of your choice. There are eight available styles including 3D animated family film, design sketch, flat vector illustration, graphic novel, maximalism, midcentury retro, photorealism, and soft digital painting.

Applying them is as straightforward as passing in an extra parameter to the Nova Canvas API. Let’s try an example.

I want to generate an image of an AWS superhero using the 3D animated family film style. To do this, I specify a taskType of "TEXT_IMAGE" and a textToImageParams object containing two parameters: text and style. The text parameter contains the prompt describing the image I want to create which in this case is “a superhero in a yellow outfit with a big AWS logo and a cape.” The style parameter specifies one of the predefined style values. I’m using "3D_ANIMATED_FAMILY_FILM" here, but you can find the full list in the Nova Canvas User Guide.

inference_params = {
   "taskType": "TEXT_IMAGE",
   "textToImageParams": {
      "text": "a superhero in a yellow outfit with a big AWS logo and a cape.",
      "style": "3D_ANIMATED_FAMILY_FILM",
   },
   "imageGenerationConfig": {
      "width": 1280,
      "height": 720,
      "seed": 321
   }
}
Python
Then, I call the invoke API just as I did in the previous example. (The code has been omitted here for brevity.) And the result? Well, I’ll let you judge for yourself, but I have to say I’m quite pleased with the AWS superhero wearing my favorite color following the 3D animated family film style exactly as I envisioned.



What’s really cool is that I can keep my code and prompt exactly the same and only change the value of the style attribute to generate an image in a completely different style. Let’s try this out. I set style to PHOTOREALISM.

inference_params = { 
   "taskType": "TEXT_IMAGE", 
   "textToImageParams": { 
      "text": "a superhero in a yellow outfit with a big AWS logo and a cape.",
      "style": "PHOTOREALISM",
   },
   "imageGenerationConfig": {
      "width": 1280,
      "height": 720,
      "seed": 7
   }
}
Python
And the result is impressive! A photorealistic superhero exactly as I described, which is a far departure from the previous generated cartoon and all it took was changing one line of code.



Things to know
Availability – Virtual try-on and style options are available in Amazon Nova Canvas in the US East (N. Virginia), Asia Pacific (Tokyo), and Europe (Ireland). Current users of Amazon Nova Canvas can immediately use these capabilities without migrating to a new model.

Pricing – See the Amazon Bedrock pricing page for details on costs.

For a preview of virtual try-on of garments, you can visit nova.amazon.com where you can upload an image of a person and a garment to visualize different clothing combinations.

If you are ready to get started, please check out the Nova Canvas User Guide or visit the AWS Console.

Matheus Guimaraes | @codingmatheus
Matheus Guimaraes
Matheus Guimaraes
Matheus Guimaraes (@codingmatheus) is a digital transformation specialist focused on AI adoption and microservices architecture. An international keynote speaker with over 20 years in tech, he’s worn many hats: from junior game programmer to CTO and tech co-founder. Matheus has helped companies of all sizes modernize and scale their systems, leading transformation programs and designing cloud-native, AI-ready architectures. Today, he shares his expertise globally through talks, blogs, and videos, passionate about helping others grow in the industry. Outside his professional life, he’s a gamer, swimmer, musician, and firm believer in the powerful intersection of creativity and technology.