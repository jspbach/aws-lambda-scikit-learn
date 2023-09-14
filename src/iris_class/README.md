# Sklearn image package for workshop

## Steps

1. `cd` into `src/`
2. Build image using the Dockerfile
3. Use image in corresponding Lambda session

## Test

Use the following JSON schema:

```json
{
	"body": {
	    "features": {
		"sepal length (cm)": 6.4,
		"sepal width": 2.9,
		"petal length": 4.3,
		"petal width": 1.3
	    }
	}
}
```

## Script

Ok, vâỵ là chúng ta đã thấy được cách để đưa một model từ thiết bị cá nhân lên cloud infrastructure với AWS Lambda và API Gateway. Hai dịch vụ này chỉ là một phần trong số rất nhiều cách để chúng ta đưa model vào sử dụng.

Nếu mọi người để ý, trong demo đầu tiên chúng ta tiến hành train lại model scikitlearn mỗi lần Lambda được gọi. Với timeout period là 15 phút và cơ sở có hạn, tất nhiên chúng ta không thể làm vậy với những mô hình phức tạp hơn.

Workflow thường thấy hơn là sẽ có model đã được train sẵn và export dưới dạng file hoặc thư mục file và artifacts. Model sẽ được đóng chung vào trong containerc, và mỗi lần chạy sẽ tương đương với một lần chúng ta load model từ artifacts đã có sẵn và thực hiện inference ngay lập tức. Như trong ví dụ tới đây, model scikit-learn của chúng ta đã được export ra một file có tên model.joblib jobli và dòng lệnh đầu tiên của Lambda khi khởi chạy là load model này vào bộ nhớ. Chúng ta có thể thấy câu lệnh này được đặt nằm ngoài `lambda-handler`, điều này là do chúng ta không muốn việc load model phải được làm lại liên tục mỗi request.

Hai ví dụ trên là với scikit-learn, tuy nhiên chúng ta hoàn toàn có thể làm tương tự với PyTorch Tensorflow, ... Hai ví dụ cũng đòi hỏi khá nhiều sự hiểu biết nhất định về cloud, vậy nên ngoài những cách này ra chúng ta cũng có thể sử dụng AWS Sagemaker, đó là giải pháp dành cho các data scientists. AWS sẽ lo những phần việc như tạo API Gateway, load balancing, server, ... nhưng đổi lại là giá của mỗi endpoint sẽ đắt hơn ít nhất 40% so với những phương pháp thủ công hơn.