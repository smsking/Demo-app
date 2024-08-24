#!/bin/bash

USERID=$(id -u)
TIMESTAMP=$(date +%F-%H-%M-%S)
SCRIPT_NAME=$(echo $0 | cut -d "." -f1)
LOGFILE=/tmp/$SCRIPT_NAME-$TIMESTAMP.log


VALIDATE(){
if [ $1 -ne 0 ]
then
    echo "$2.....Failure" 
    exit 1
else
    echo "$2.....Success" 
fi
}


if [ $USERID -ne 0 ]
then
    echo "Run the Script with Root user"
    exit 1 #Manually exit if error comes.
else
    echo "Your super user."
fi

dnf install mysql-server -y &>>$LOGFILE
VALIDATE $? "Installing My SQL" 

systemctl enable mysqld &>>$LOGFILE
VALIDATE $? "Enabling My SQL Server" 

systemctl start mysqld &>>$LOGFILE
VALIDATE $? "Starting My SQL Server" 

mysql_secure_installation --set-root-pass ExpenseApp@1 &>>$LOGFILE
VALIDATE $? "setting up root password" 





