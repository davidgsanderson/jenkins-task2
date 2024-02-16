# Use Python 3.6 or later as a base image
FROM python:latest

# Copy contents into image
COPY . .
 
# Install pip dependencies from requirements
RUN pip install -r requirements.txt

# Set YOUR_NAME environment variable
ARG YOUR_NAME QA
ENV MYSQL_ROOT_PASSWORD=password 

# Expose the correct port
EXPOSE 5000

# Create an entrypoint
ENTRYPOINT [ "python","app.py" ]
