---
title: "Blog 1"
date: 2025-09-10
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Cập nhật Amazon Nova Canvas: Virtual try-on và style options hiện đã khả dụng
Tác giả: Matheus Guimaraes | vào ngày 02 tháng 07 năm 2025 | chuyên mục  Amazon Bedrock, Amazon Nova, Announcements, Launch, News, Retail Permalink | Comments | Share

Bạn đã bao giờ ước rằng mình có thể nhanh chóng hình dung một bộ trang phục mới sẽ trông như thế nào trên người trước khi mua? Hoặc một món đồ nội thất sẽ trông ra sao trong phòng khách của bạn? Hôm nay, chúng tôi rất vui mừng giới thiệu một khả năng mới đó là virtual try-on trong Amazon Nova Canvas, giúp điều đó trở thành hiện thực. Ngoài ra, chúng tôi cũng bổ sung tám tùy chọn phong cách mới nhằm cải thiện tính nhất quán khi tạo hình ảnh từ văn bản. Những tính năng này mở rộng khả năng tạo ảnh bằng AI của Nova Canvas, giúp việc tạo ra các hình ảnh sản phẩm chân thực và hình ảnh cách điệu trở nên dễ dàng hơn bao giờ hết, từ đó nâng cao trải nghiệm của khách hàng.

Hãy cùng xem nhanh cách bạn có thể bắt đầu sử dụng những tính năng này ngay hôm nay.

Bắt đầu

Điều đầu tiên là hãy đảm bảo rằng bạn có quyền truy cập vào mô hình Nova Canvas thông qua các cách thông thường. Hãy vào Amazon Bedrock console, chọn Model access và bật Amazon Nova Canvas cho tài khoản của bạn, đồng thời đảm bảo rằng bạn đã chọn đúng region phù hợp với khối lượng công việc của mình. Nếu bạn đã có quyền truy cập và đang sử dụng Nova Canvas, bạn có thể bắt đầu sử dụng các tính năng mới ngay lập tức vì chúng đã được tự động kích hoạt cho bạn.

Virtual try-on

Tính năng mới thú vị đầu tiên là virtual try-on. Với tính năng này, bạn có thể tải lên hai bức ảnh và yêu cầu Amazon Nova Canvas ghép chúng lại với nhau để tạo ra kết quả chân thực. Những bức ảnh này có thể là quần áo, phụ kiện, đồ nội thất, hoặc bất kỳ sản phẩm nào khác, bao gồm cả thời trang. Ví dụ, bạn có thể cung cấp ảnh của một người làm source image và ảnh một bộ trang phục làm reference image. Khi đó, Amazon Nova Canvas sẽ tạo ra một bức ảnh mới trong đó chính người đó đang mặc bộ trang phục. Hãy thử điều này nhé!

Điểm khởi đầu của tôi là chọn hai bức ảnh. Tôi đã chọn một tấm hình của bản thân trong tư thế mà tôi nghĩ sẽ phù hợp cho việc đổi trang phục, và một tấm hình của chiếc hoodie có thương hiệu AWS.

Lưu ý rằng Nova Canvas chỉ chấp nhận hình ảnh có tối đa 4,1 triệu pixel – tương đương với 2.048 x 2.048 – vì vậy hãy đảm bảo bạn đã điều chỉnh kích thước ảnh để phù hợp với giới hạn này nếu cần. Ngoài ra, nếu bạn muốn chạy mã Python trong bài viết này, hãy đảm bảo rằng bạn đã cài đặt Python 3.9 hoặc phiên bản mới hơn, cùng với các gói boto3 và pillow.

Để thử áo hoodie trên bức ảnh của tôi, tôi sử dụng Amazon Bedrock Runtime invoke API. Bạn có thể tìm thấy toàn bộ chi tiết về cấu trúc request và response cho API này trong Amazon Nova User Guide. Mã nguồn khá đơn giản, chỉ cần một vài inference parameters. Tôi sử dụng giá trị mới cho taskType là "VIRTUAL_TRY_ON". Sau đó, tôi chỉ định các thiết lập mong muốn, bao gồm cả source image và reference image, bằng cách sử dụng đối tượng virtualTryOnParams để thiết lập một số tham số bắt buộc. Lưu ý rằng cả hai hình ảnh đều phải được chuyển đổi sang chuỗi Base64.

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

Nova Canvas sử dụng masking để thao tác với hình ảnh. Đây là một kỹ thuật cho phép AI tập trung tạo hình ảnh ở những khu vực cụ thể trong ảnh trong khi vẫn giữ nguyên các phần khác, tương tự như việc dùng băng keo giấy để che đi những chỗ bạn không muốn sơn.

Bạn có thể sử dụng ba chế độ masking khác nhau, được chọn thông qua việc thiết lập maskType với giá trị phù hợp. Trong ví dụ này, tôi dùng "GARMENT", yêu cầu tôi chỉ định phần nào trên cơ thể sẽ được mask. Tôi chọn "UPPER_BODY", nhưng bạn cũng có thể dùng các lựa chọn khác như "LOWER_BODY", "FULL_BODY", hoặc "FOOTWEAR" nếu muốn tập trung vào bàn chân. Xem thêm danh sách đầy đủ các tùy chọn trong tài liệu hướng dẫn.

Sau đó, tôi gọi invoke API, truyền vào các inference arguments này và lưu lại ảnh đã được tạo ra vào ổ đĩa.

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


Kết quả tôi nhận được thật ấn tượng!



Và chỉ như vậy thôi, tôi đã trở thành chủ nhân của một chiếc hoodie có logo AWS!

Ngoài mask type "GARMENT", bạn cũng có thể dùng "PROMPT" hoặc "IMAGE". Với "PROMPT", bạn vẫn cung cấp source image và reference image, nhưng đồng thời đưa vào một natural language prompt để chỉ định phần nào của source image mà bạn muốn thay thế. Cách này tương tự như khi thực hiện các tác vụ "INPAINTING" và "OUTPAINTING" trong Nova Canvas. Nếu bạn muốn sử dụng chính image mask của mình, hãy chọn mask type "IMAGE" và cung cấp một ảnh trắng-đen để làm mask. Trong đó, vùng màu đen thể hiện những pixel bạn muốn thay thế trong source image, còn vùng màu trắng là những phần bạn muốn giữ lại.

Khả năng này đặc biệt hữu ích cho các nhà bán lẻ. Họ có thể dùng nó để giúp khách hàng đưa ra quyết định mua sắm tốt hơn bằng cách trực tiếp xem sản phẩm sẽ trông như thế nào trước khi mua.

Sử dụng tùy chọn style

Tôi luôn tự hỏi không biết trông mình sẽ thế nào nếu là một siêu anh hùng anime. Trước đây, tôi có thể dùng Nova Canvas để chỉnh sửa một bức ảnh của mình, nhưng phải dựa khá nhiều vào kỹ năng prompt engineering để có được kết quả ưng ý. Giờ đây, Nova Canvas đã có sẵn các pre-trained styles mà bạn có thể áp dụng trực tiếp vào ảnh để tạo ra kết quả chất lượng cao, đúng theo phong cách nghệ thuật mà bạn lựa chọn. Hiện có tám style có sẵn, bao gồm 3D animated family film, design sketch, flat vector illustration, graphic novel, maximalism, midcentury retro, photorealism, và soft digital painting.


Việc áp dụng chúng rất đơn giản, chỉ cần truyền thêm một parameter vào Nova Canvas API. Hãy thử một ví dụ.

Tôi muốn tạo ra một hình ảnh siêu anh hùng AWS theo phong cách 3D animated family film. Để làm điều này, tôi chỉ định taskType là "TEXT_IMAGE" và một textToImageParams object gồm hai parameters là text và style. Text là prompt mô tả hình ảnh tôi muốn tạo trong trường hợp này là “một siêu anh hùng trong bộ trang phục màu vàng với logo AWS lớn và một chiếc áo choàng”. Style được dùng để chỉ định một trong những predefined style values. Ở đây, tôi dùng "3D_ANIMATED_FAMILY_FILM", nhưng bạn có thể tìm toàn bộ danh sách trong Nova Canvas User Guide.

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


Sau đó, tôi gọi invoke API giống như ở ví dụ trước. (Phần mã nguồn đã được lược bỏ để cho ngắn gọn.) Và kết quả là? Tôi sẽ để bạn tự đánh giá, nhưng cá nhân tôi phải nói rằng tôi rất hài lòng với hình ảnh siêu anh hùng AWS mặc bộ đồ màu yêu thích của tôi, theo đúng phong cách 3D animated family film như tôi đã hình dung.



Điều thực sự thú vị là tôi có thể giữ nguyên toàn bộ code và prompt, chỉ cần thay đổi giá trị của thuộc tính style là đã có thể tạo ra một hình ảnh theo phong cách hoàn toàn khác. Hãy thử nhé. Tôi đặt style thành PHOTOREALISM.

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


Và kết quả thật ấn tượng! Một siêu anh hùng photorealistic chính xác như tôi đã mô tả, khác hẳn so với hình ảnh hoạt hình được tạo ra trước đó và tất cả chỉ cần thay đổi một dòng code.



Những điều cần biết

Availability – Tính năng Virtual try-on và style options hiện có sẵn trong Amazon Nova Canvas tại các khu vực US East (N. Virginia), Asia Pacific (Tokyo), và Europe (Ireland). Người dùng hiện tại của Amazon Nova Canvas có thể sử dụng ngay các tính năng này mà không cần chuyển sang một model mới.

Pricing – Xem chi tiết về chi phí tại trang Amazon Bedrock pricing.

Để xem thử tính năng virtual try-on cho quần áo, bạn có thể truy cập nova.amazon.com, nơi bạn có thể tải lên hình ảnh của một người và một trang phục để trực quan hóa các sự kết hợp khác nhau.

Nếu bạn đã sẵn sàng để bắt đầu, vui lòng tham khảo Nova Canvas User Guide hoặc truy cập AWS Console.

Matheus Guimaraes | @codingmatheus

Matheus Guimaraes

Matheus Guimaraes (@codingmatheus) là chuyên gia về .NET và microservices, international keynote speaker, và Developer Advocate tại AWS. Với hơn 25 năm trong lĩnh vực công nghệ, anh đã đảm nhận nhiều vai trò khác nhau — từ junior game programmer đến CTO và startup founder. Matheus đã giúp nhiều công ty ở mọi quy mô modernize và scale hệ thống của họ, dẫn dắt digital transformations và thiết kế cloud-native 
architectures. Hiện tại, anh chia sẻ kiến thức của mình trên toàn cầu thông qua các buổi talks, blogs, và videos, với niềm đam mê giúp người khác phát triển trong ngành. Ngoài công nghệ, anh còn là gamer, swimmer, musician, và tin vào sức mạnh của sự giao thoa giữa creativity và code.




