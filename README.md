#!/bin/bash

# Assuming your XML content is in a file named config.xml
file="config.xml"

# Extract username and password values from non-commented lines
username=$(grep -oP '^[^#]*(?!<!--)<property name="username" value="([^"]*)"' "$file" | awk -F '"' '{print $4}')
password=$(grep -oP '^[^#]*(?!<!--)<property name="password" value="([^"]*)"' "$file" | awk -F '"' '{print $4}')

echo "Username: $username"
echo "Password: $password"

