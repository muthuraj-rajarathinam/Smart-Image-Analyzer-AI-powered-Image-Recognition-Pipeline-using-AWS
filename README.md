# Smart-Image-Analyzer-AI-powered-Image-Recognition-Pipeline-using-AWS
A serverless AI image recognition system built on AWS that lets users upload images through a web interface, automatically analyzes the image using Amazon Rekognition, and displays detected labels in real-time — all without managing any servers.


# 📸 Smart Image Analyzer — AI-Powered Serverless Image Label Detection

This project is a full-stack, serverless image recognition pipeline built entirely on AWS. It allows users to upload images via a simple HTML frontend. Uploaded images are automatically analyzed using **Amazon Rekognition**, and the detected labels (objects, animals, scenes, etc.) are stored in **DynamoDB**. The frontend then fetches the results and displays them in real-time.


## 🔥 Why This Project Stands Out
```
- 🔁 **Event-Driven Architecture** — Triggered actions on S3 uploads using Lambda
- ☁️ **Serverless & Scalable** — No servers to manage or maintain
- 🤖 **AI-Powered** — Leverages Amazon Rekognition for real-time image analysis
- ⚡ **Frontend Integration** — Automatically displays labels after upload
- 💾 **DynamoDB Storage** — Persistent storage of results for fast retrieval

```
## 🧠 Use Case

A user uploads an image using the frontend. The system:
1. Uploads the image to Amazon S3 using a pre-signed URL
2. Automatically triggers a Lambda function upon upload
3. That Lambda runs Rekognition to detect labels in the image
4. Detected labels are saved in a DynamoDB table
5. The frontend waits and automatically fetches results using an API Gateway endpoint

---

## 📌 Technologies Used
```

| Layer        | Services Used                                |
|--------------|----------------------------------------------|
| Frontend     | HTML, CSS, JavaScript                        |
| Serverless   | AWS Lambda                                   |
| AI/ML        | Amazon Rekognition                           |
| Storage      | Amazon S3 (image storage), DynamoDB (labels) |
| APIs         | Amazon API Gateway (HTTP APIs)            |
| Permissions  | IAM Roles and Policies                       |

```
## 🧱 Architecture Diagram

```

\[ HTML + JS Frontend ]
|
↓
\[ API Gateway - GET /PresignedURL ]
↓
\[ Lambda - GeneratePreSignedURL ]
↓
\[ Amazon S3 - Image Stored ]
↓ (trigger)
\[ Lambda - ProcessUploadedImage ]
↓
\[ Rekognition - Label Detection ]
↓
\[ DynamoDB - Store Detected Labels ]
↑
\[ API Gateway - GET /GetImageLabels ]
↑
\[ Lambda - GetImageLabels ]
↑
\[ HTML displays results automatically ]

```

---

## 🌐 Live Flow Example

1. User uploads `funny-dog.jpg`
2. Image is sent to S3 via pre-signed URL
3. S3 triggers `ProcessUploadedImage` Lambda
4. Rekognition returns:
```

\["Dog", "Pet", "Animal", "Grass", "Canine"]

````
5. Labels are saved to DynamoDB:
```json
{
  "filename": "funny-dog.jpg",
  "labels": ["Dog", "Pet", "Animal", "Grass", "Canine"]
}
````
7. Frontend waits 4 seconds, then fetches labels using & Detected labels are displayed under the image

```
Detected Labels : "Dog", "Pet", "Animal", "Grass", "Canine"


```
## 🛠️ Project Setup — Step-by-Step

### 1. 🪣 Set Up S3

* Create a bucket (e.g., `smart-image-analyzer`)
* Enable public access (for preview)
* Enable event notifications (on `PUT`) to trigger `ProcessUploadedImage` Lambda

### 2. 🔐 IAM Roles

* Create a role with permissions for:

  * `AmazonS3FullAccess`
  * `AmazonRekognitionFullAccess`
  * `AmazonDynamoDBFullAccess`
  * `CloudWatchLogsFullAccess`
add inline policy :
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "AllowCloudWatchLogs",
			"Effect": "Allow",
			"Action": [
				"logs:CreateLogGroup",
				"logs:CreateLogStream",
				"logs:PutLogEvents"
			],
			"Resource": "*"
		}
	]
}

Attach this role to your Lambda functions.

### 3. 📸 Amazon Rekognition

* No setup needed — just make sure images are in S3 and permissions are granted
* Rekognition will detect up to 10 labels per image with ≥ 75% confidence

### 4. 🧠 Create DynamoDB Table

* Table name: `ImageLabels`
* Primary key: `filename` (String)

### 5. 🧩 Lambda Functions(attach IAM role To Every Function)

```Create three functions:

#### a. `GeneratePresignedURL`

* Returns a secure upload URL
* Triggered by frontend before upload

#### b. `ProcessUploadedImage`

* Triggered automatically when image is uploaded to S3
* Calls Rekognition
* Saves labels to DynamoDB

#### c. `GetImageLabels`

* API to retrieve labels for a given filename
* Called by frontend after upload

```
### 6. 🌐 API Gateway

Create two GET routes(For genereatepresignedurl & getimagelables) \Get Method:

* `/PresignedURL?filename=...`
* `/GetImageLabels?filename=...`

Enable **CORS** on both

---

## 🧪 How to Use

1. Open `index.html` in your browser
2. Select an image (e.g., `dog.jpg`)
3. Click **Upload**
4. Wait 4–5 seconds
5. Labels will appear below the image

---

## 🧾 Sample Output

```json
{
  "filename": "dog.jpg",
  "labels": ["Dog", "Pet", "Mammal", "Animal"]
}
```

---

## 🎯 Future Enhancements

* Add **face detection** (age, emotion, gender)
* Display Rekognition confidence scores
* Search and browse all past uploads
* Admin dashboard with visual charts (Chart.js, D3.js)
* Allow batch uploads and async processing
* User authentication (e.g., Cognito)

---

## 🙋‍♂️ Author
**Muthuraj Rajarathinam**
🔗 [GitHub Profile](https://github.com/muthuraj-rajarathinam)
🌐 [Portfolio](https://www.datascienceportfol.io/muthuraj)


## 🤝 Contributions

Pull requests are welcome! Feel free to fork, improve, or expand this system.
