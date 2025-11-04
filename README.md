# mlops-winequality

Dự án **mlops-winequality** triển khai quy trình **MLOps hoàn chỉnh** cho bài toán dự đoán **chất lượng rượu vang đỏ**. Hệ thống sử dụng mô hình **ElasticNet**, được tích hợp **MLflow** để theo dõi thí nghiệm, **Flask** để phục vụ dự đoán, **Docker** để đóng gói, và **GitHub Actions + AWS ECR** để tự động hóa CI/CD.

---

## 1. Tổng quan

* **Ngôn ngữ:** Python 3.11
* **Framework:** scikit-learn, Flask, MLflow
* **Cơ sở hạ tầng:** Docker, GitHub Actions, AWS ECR
* **Pipeline:** gồm 5 giai đoạn từ ingestion đến evaluation

> 🔗 [Xem mã nguồn đầy đủ](https://github.com/FuuToru/MLOPs-winequality)

---

## 2. Kiến trúc & Pipeline

Pipeline được điều phối trong [`main.py`](https://github.com/FuuToru/MLOPs-winequality/blob/main/main.py) và bao gồm:

| Giai đoạn           | Mô tả                                  | Mã nguồn                                                                                                                         |
| ------------------- | -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Data Ingestion      | Tải và giải nén dữ liệu                | [data_ingestion.py](https://github.com/FuuToru/MLOPs-winequality/blob/main/src/mlProject/components/data_ingestion.py)           |
| Data Validation     | Kiểm tra schema, ghi trạng thái hợp lệ | [data_validation.py](https://github.com/FuuToru/MLOPs-winequality/blob/main/src/mlProject/components/data_validation.py)         |
| Data Transformation | Tách dữ liệu train/test                | [data_transformation.py](https://github.com/FuuToru/MLOPs-winequality/blob/main/src/mlProject/components/data_transformation.py) |
| Model Trainer       | Huấn luyện ElasticNet, lưu model       | [model_trainer.py](https://github.com/FuuToru/MLOPs-winequality/blob/main/src/mlProject/components/model_trainer.py)             |
| Model Evaluation    | Tính toán RMSE, MAE, R², log MLflow    | [model_evaluation.py](https://github.com/FuuToru/MLOPs-winequality/blob/main/src/mlProject/components/model_evaluation.py)       |

Giao diện web sử dụng [`app.py`](https://github.com/FuuToru/MLOPs-winequality/blob/main/app.py) với các route:

* `/` – giao diện nhập dữ liệu.
* `/predict` – nhận dữ liệu POST và trả kết quả.
* `/train` – kích hoạt lại toàn bộ pipeline.

---

## 3. Cài đặt

```bash
git clone https://github.com/FuuToru/MLOPs-winequality.git
cd MLOPs-winequality
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

---

## 4. Chạy pipeline và web app

Huấn luyện pipeline:

```bash
python main.py
```

Chạy ứng dụng web Flask:

```bash
python app.py  # mở tại http://localhost:8080
```

---

## 5. Theo dõi thí nghiệm với MLflow

* Kết quả được lưu tại thư mục `mlruns/`.
* Mở giao diện theo dõi:

  ```bash
  mlflow ui --backend-store-uri mlruns --port 5000
  ```

  Truy cập [http://localhost:5000](http://localhost:5000) để xem kết quả huấn luyện.

---

## 6. Docker hóa

* **Build image:** `docker build -t winequality:latest .`
* **Chạy container:** `docker run -p 8080:8080 winequality:latest`

---

## 7. CI/CD với GitHub Actions + AWS ECR

Workflow CI/CD: [.github/workflows/main.yaml](https://github.com/FuuToru/MLOPs-winequality/blob/main/.github/workflows/main.yaml)

### Quy trình chính

1. **Integration:** kiểm thử và lint.
2. **Build & Push:** build image và đẩy lên AWS ECR.
3. **Deployment:** runner self-hosted pull image và khởi chạy container.

---

## 8. Cấu trúc thư mục

> 🔗 [Xem chi tiết tại đây](https://github.com/FuuToru/MLOPs-winequality/tree/main)

```text
fuutoru-mlops-winequality/
├── app.py               
├── main.py              
├── src/mlProject/       
├── config/              
├── templates/           
├── artifacts/           
├── Dockerfile
├── requirements.txt
└── .github/workflows/
```

---

## 9. Giấy phép

Phân phối theo **Apache License 2.0**.
Xem chi tiết tại [LICENSE](https://github.com/FuuToru/MLOPs-winequality/blob/main/LICENSE).
