#!/bin/bash

# Source directory
source_dir="/Sagar/logs/"

# Destination directory
backup_dir="/backup/Sagar/logs/"

# Create a timestamp for the backup folder name (format: YYYY-MM-DD-HHMMSS)
timestamp=$(date +'%Y-%m-%d-%H%M%S')

# Create the backup directory with the current timestamp
backup_path="$backup_dir$timestamp"
mkdir -p "$backup_path"

# Copy all files from source directory to backup directory
cp -r "$source_dir"* "$backup_path"

echo "Backup completed to $backup_path"
