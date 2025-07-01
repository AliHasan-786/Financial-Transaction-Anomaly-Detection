# Financial Transaction Anomaly Detection
This project demonstrates the end-to-end development of a scalable transaction
anomaly detection platform, leveraging AWS cloud services for data ingestion,
storage, processing, and analysis. It aims to identify fraudulent or unusual
credit card transactions, providing insights crucial for financial institutions.

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Architecture](#architecture)
- [Technologies Used](#technologies-used)
- [Data Source](#data-source)
- [Setup and Installation](#setup-and-installation)
- [Usage](#usage)
- [Visualizations](#visualizations)
- [Anomaly Detection Methodology](#anomaly-detection-methodology)
- [Future Enhancements](#future-enhancements)
- [Contact](#contact)

## Features
- **Automated Data Ingestion**: Seamlessly ingests large volumes of credit card
transaction data.
- **Scalable Data Storage**: Utilizes AWS RDS for structured transaction data
and DynamoDB for semi-structured behavioral data.
- **Robust ETL Pipeline**: Python-based ETL scripts for data cleaning,
transformation, and loading into respective databases.
- **Advanced Anomaly Detection**: Implements unsupervised machine learning
models (e.g., Isolation Forest) to identify anomalous transactions.
- **Interactive Data Visualizations**: Provides dynamic dashboards and stories
in Tableau Public for exploring transaction patterns and anomalies.
- **Cloud-Native Architecture**: Deployed entirely on AWS Free Tier services,
demonstrating cloud proficiency.

## Architecture
Below is a high-level overview of the Financial Transaction Anomaly Detection architecture:
*Brief explanation of the diagram and data flow.*

## Technologies Used
- **Cloud Platform**: AWS (S3, RDS PostgreSQL, DynamoDB, EC2)
- **Programming Languages**: Python
- **Libraries**: pandas, numpy, scikit-learn, psycopg2-binary, boto3
- **Database**: PostgreSQL, DynamoDB
- **Data Visualization**: Tableau Public
- **Version Control**: Git, GitHub

## Data Source
The project utilizes the [Credit Card Transactions Dataset]
([https://www.kaggle.com/datasets/priyamchoksi/credit-card-transactions](https://www.kaggle.com/datasets/priyamchoksi/credit-card-transactions-dataset?resource=download)) from
Kaggle, an anonymized dataset simulating real-world transaction patterns. This
dataset provides rich features essential for financial analysis and anomaly
detection.

## Setup and Installation
To set up this project locally and on AWS, follow these steps:
1. **Clone the Repository**:
```bash
git clone https://github.com/AliHasan-786/Financial-Transaction-Anomaly-Detection.git
cd Financial-Transaction-Anomaly-Detection
```
2. **AWS Account and CLI Configuration**:
Ensure you have an AWS account and the AWS CLI configured with appropriate
credentials. Refer to the [AWS CLI documentation]
(https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) for
setup.
3. **Download Dataset**:
Download the `credit_card_transactions.csv` from the [Kaggle dataset page]
(https://www.kaggle.com/datasets/priyamchoksi/credit-card-transactions) and
place it in the `data/` directory.
4. **AWS Infrastructure Deployment**:
* **S3 Bucket**: Create an S3 bucket (e.g., `yourname-Financial-Transaction-Anomaly-Detection-data`) and upload `credit_card_transactions.csv` to it.
* **RDS PostgreSQL Database**: Launch an RDS PostgreSQL instance
(`db.t2.micro`, Free Tier eligible) with a specified database name, username,
and password. Configure its security group to allow inbound connections from
your EC2 instance.
* **DynamoDB Table**: Create a DynamoDB table named
`CapitalOneGuard_CustomerBehavior` with `customer_id` as the partition key.
* **EC2 Instance**: Launch an EC2 `t2.micro` instance (e.g.,
`CapitalOneGuard-Analytics`). Configure its security group to allow SSH from
your IP and outbound connections to your RDS instance.
5. **EC2 Instance Setup**:
SSH into your EC2 instance and install necessary dependencies:
```bash
sudo yum update -y # For Amazon Linux
# OR
sudo apt update && sudo apt install -y python3-pip # For Ubuntu
pip install pandas numpy scikit-learn psycopg2-binary boto3
```
6. **Run ETL Pipeline**:
Copy the `etl_scripts/etl_script.py` to your EC2 instance. Update the
`RDS_HOST`, `RDS_DBNAME`, `RDS_USER`, `RDS_PASSWORD`, and `REGION_NAME`
variables in the script with your AWS details. Then execute:
```bash
python etl_script.py
```
This will load data into your RDS and DynamoDB instances.
7. **Run Anomaly Detection (Optional)**:
If you have a separate anomaly detection script (e.g.,
`analysis_scripts/anomaly_detection.py`), copy it to EC2 and run it to generate
`anomaly_score` and `is_anomaly` fields.

## Usage
Once the data is loaded and processed, you can:
- **Query Data**: Connect to your RDS PostgreSQL database using SQL clients
(e.g., DBeaver, pgAdmin) to perform ad-hoc queries on transaction data.
- **Analyze Behavioral Data**: Use the AWS CLI or boto3 in Python to query
customer behavioral data stored in DynamoDB.
- **Explore Visualizations**: Access the interactive Tableau Public dashboards
to explore transaction trends, anomaly distributions, and identify potential
fraud patterns.

## Visualizations
Interactive dashboards and stories are available on Tableau Public, providing
deep insights into transaction patterns and anomalies. Click the link below to
explore the live dashboards:
([View Live Tableau Public Dashboard](https://public.tableau.com/views/FinancialTransactionAnomalyDetection/Dashboard1?:language=en-GB&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link))
Here are some key visualizations from the project:
### Daily Trend of Anomalous Transactions
![Daily Trend of Anomalous Transactions]
(/home/ubuntu/daily_anomalies_trend.png)
*This visualization highlights the daily count of anomalous transactions,
revealing weekly patterns and spikes in suspicious activity.*
### Anomaly Score Distribution by Category
![Anomaly Score by Category](/home/ubuntu/anomaly_score_by_category.png)
*This chart identifies transaction categories with the highest average anomaly
scores, pinpointing high-risk areas.*
### Geographical Distribution of Anomalous Transactions
![Geographical Distribution of Anomalies](/home/ubuntu/anomalies_by_state.png)
*A map illustrating the geographical spread of anomalous transactions, useful
for identifying regional fraud hotspots.*
### Transaction Amount vs. Anomaly Score
![Transaction Amount vs. Anomaly Score]
(/home/ubuntu/amt_vs_anomaly_score_scatter.png)
*This scatter plot visualizes the relationship between transaction amount and
anomaly score, clearly distinguishing anomalous transactions.*

## Anomaly Detection Methodology
The project employs an unsupervised anomaly detection approach using the
Isolation Forest algorithm from `scikit-learn`. Key features such as
transaction amount, time-based features (hour of day, day of week), and
aggregated customer spending patterns are used to train the model. The model
assigns an `anomaly_score` to each transaction, with a binary `is_anomaly` flag
indicating potential fraudulent activity.

## Future Enhancements
- Implement real-time data streaming using AWS Kinesis or Kafka for immediate
anomaly detection.
- Integrate with AWS Lambda for serverless execution of anomaly detection
models.
- Develop a user interface (e.g., using Flask or Streamlit) to interact with
the platform and view real-time alerts.
- Explore more advanced machine learning models for anomaly detection, such as
deep learning approaches.
- Incorporate external data sources (e.g., IP geolocation, device information)
to enrich transaction data.
