#Logging Microservice for Package Tracker

#Micoservice logs message from application to log file. This microservice monitors text file for log requests, processes, and stores in a log file. 

#Communication Contract

How to Request Data:
Logging microservice accepts requests via text file communication pipe. 
Log a message, program write io/logg-service.txt file with specific format. 
Request format: Message must start with prefix LOG| followed by text to log. 

Microservice will:
*Monitor io/logger-service.txt file
*If a message is detected, extract the contact after prefix LOG|
*Add timestamp
*Add formatted message to central log file io/log.txt file
*Delete request file to show successful processing

Example:
def send_log(message, service_file="io/logger-service.txt"):
    """Send a log message to the microservice"""
    try:
        #Create directory if it doesn't exist
        import os
        os.makedirs(os.path.dirname(service_file), exist_ok=True)
        
        #Write the log request to the service file
        with open(service_file, "w") as file:
            file.write(f"LOG|{message}")
        return True
    except Exception as e:
        print(f"Logging failed: {e}")
        return False
example:
send_log("Package PKG123456 created")


How to Receive Data:
Logging microservice stores all log entries in central log file. Logged data is from io/log.txt file. 

Example:
def read_logs(log_file="io/log.txt"):
    """Read all logs from the log file"""
    import os
    if not os.path.exists(log_file):
        return []
        
    try:
        with open(log_file, "r") as file:
            logs = file.readlines()
        return [log.strip() for log in logs]
    except Exception as e:
        print(f"Error reading logs: {e}")
        return []

example:
logs = read_logs()
for log in logs:
    print(log)


UML sequence diagram
![uml-sequence-diagram](https://github.com/user-attachments/assets/0144e2de-ae1d-4968-b945-7c2b4f8669a0)
