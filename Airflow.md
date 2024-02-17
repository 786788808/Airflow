# Airflow
### Airflow Components
Airflow is organized into three main components
- The Airflow scheduler(Airflow 调度器)
- The Airflow workers
- The Airflow webserver(Airflow Web 服务器)

示例一:  
```python
import json
import pathlib

import airflow
import requests
import requests.exceptions as requests_exceptions
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator

dag = DAG(  # 实例化一个DAG对象，dag是DAG类的实例名称，可以改成其他名字，such as demo_dag,在operator里，会引用到这个名字，dag=xxx_dag_name,根据实际情况改就行了。DAG类有两个必要的参数，一个是dag_id,另一个是start_date。
    dag_id="dag_demo",  # DAG的名称，在Airflow 用户界面看到的DAG的名称
    start_date=airflow.utils.dates.days_ago(8),  # 设置DAG首次运行的日期和时间
    schedule_interval=None,  # 这不是必要的参数。当前设置为None，代表DAG不会自动运行，要从Airflow UI 去手动打开，然后trigger，运行。
)

download_step = BashOperator(
    task_id="download_step",
    bash_command='echo "first step"',
    dag=dag,
)

def _webs_pictures():
    pass

parse_pictures = PythonOperator(
    task_id="parse_pictures", python_callable=_webs_pictures, dag=dag
)

show_result = BashOperator(
    task_id="show_result",
    bash_command='echo "third task"',
    dag=dag,
)

download_step >> parse_pictures >> show_result
```
