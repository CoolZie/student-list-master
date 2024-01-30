FROM python:3.8-buster
LABEL name="Sidiki"
RUN apt update -y && apt install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y
RUN pip install flask==2.0.0 flask_httpauth==4.1.0 flask_simpleldap python-dotenv==0.14.0
COPY ./requirements.txt /requirements.txt
COPY ./student_age.py /student_age.py
RUN pip3 install -r /requirements.txt
EXPOSE 5000
CMD [ "python", "./student_age.py" ]

