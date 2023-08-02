# -*- coding: utf-8 -*-
"""
Created on Wed Aug  2 18:31:38 2023

@author: GeorgeIU
"""

import streamlit as st
import requests
import psycopg2
from psycopg2 import sql

# PostgreSQL database credentials
HOST = "121.202.203.180"
PORT = "7496"
DATABASE = "biotime"
USER = "postgres"
PASSWORD = ""

# API endpoint and headers
API_URL = "http://121.202.203.180:85/personnel/api/employees/"
HEADERS = {"Authorization": "JWT eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6ImNhZXNhcjYxMTI0MTA4QGdtYWlsLmNvbSIsImV4cCI6MTY5MTU3MzA1MywiZW1haWwiOiJjYWVzYXI2MTEyNDEwOEBnbWFpbC5jb20iLCJvcmlnX2lhdCI6MTY5MDk2ODI1M30.niZL_zUxn8YJfXO9SCS2DY_HruVxDeNRAGCMuk8UE20"}

# Connect to the database
conn = psycopg2.connect(
    host=HOST,
    port=PORT,
    dbname=DATABASE,
    user=USER,
    password=PASSWORD
)

# Get the next unique emp_code
def get_next_emp_code():
    with conn.cursor() as cursor:
        cursor.execute("SELECT MAX(emp_code) FROM public.personnel_employee WHERE emp_code LIKE '500%'")
        result = cursor.fetchone()
        max_emp_code = result[0] if result else None
        next_emp_code = str(int(max_emp_code) + 1) if max_emp_code else "500001"
        return next_emp_code

# Streamlit app
st.title('User Registration')

first_name = st.text_input('First Name')
last_name = st.text_input('Last Name')

if st.button('Submit'):
    emp_code = get_next_emp_code()
    data = {
        "emp_code": emp_code,
        "department": 7,
        "area": [3],
        "first_name": first_name,
        "last_name": last_name,
        "verify_mode": 0
    }
    response = requests.post(API_URL, headers=HEADERS, json=data)
    if response.status_code == 201:
        st.success(f"User {first_name} {last_name} registered successfully with emp_code {emp_code}.")
    else:
        st.error("Failed to register user.")
