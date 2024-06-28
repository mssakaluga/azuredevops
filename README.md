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

================================

if [ "#{patchfile}" == "misc_25687_ODA_V403_1719472889.tar.gz" ]
then
if [ -d #{tomcat_install_loc}/#{tomcatfolder}/webapps/blazon/ ]
then
backup_path="#{backuploc}/#{tomcatfolder}_$(date +'%Y-%m-%d-%H%M%S')"
mkdir -p "#{backup_path}/blazon"
cp -arpf #{tomcat_install_loc}/#{tomcatfolder}/webapps/blazon/* #{backup_path}/blazon/
if [ -d #{backup_path}/blazon/ ]
then
rm -rf #{tomcat_install_loc}/#{tomcatfolder}/webapps/blazon/
fi
fi
if [ -d #{tomcat_install_loc}/#{tomcatfolder}/webapps/oda/ ]
then
mkdir -p "#{backup_path}/oda"
cp -arpf #{tomcat_install_loc}/#{tomcatfolder}/webapps/blazon/* #{backup_path}/oda/
if [ -d #{backup_path}/oda/ ]
then
rm -rf #{tomcat_install_loc}/#{tomcatfolder}/webapps/oda/
fi
fi
fi



patchfile = "misc_25687_ODA_V403_1719472889.tar.gz"
tomcat_install_loc = "/path/to/tomcat/installation"
tomcatfolder = "your_tomcat_folder"
backuploc = "/path/to/backup/location"

# Define the bash script as a string
bash_script = <<~BASH
  if [ "#{patchfile}" == "misc_25687_ODA_V403_1719472889.tar.gz" ]; then
    if [ -d "#{tomcat_install_loc}/#{tomcatfolder}/webapps/blazon/" ]; then
      backup_path="#{backuploc}/#{tomcatfolder}_$(date +'%Y-%m-%d-%H%M%S')"
      mkdir -p "#{backup_path}/blazon"
      cp -arpf "#{tomcat_install_loc}/#{tomcatfolder}/webapps/blazon/"* "#{backup_path}/blazon/"
      if [ -d "#{backup_path}/blazon/" ]; then
        rm -rf "#{tomcat_install_loc}/#{tomcatfolder}/webapps/blazon/"
      fi
    fi
    if [ -d "#{tomcat_install_loc}/#{tomcatfolder}/webapps/oda/" ]; then
      mkdir -p "#{backup_path}/oda"
      cp -arpf "#{tomcat_install_loc}/#{tomcatfolder}/webapps/oda/"* "#{backup_path}/oda/"
      if [ -d "#{backup_path}/oda/" ]; then
        rm -rf "#{tomcat_install_loc}/#{tomcatfolder}/webapps/oda/"
      fi
    fi
  fi
BASH

# Execute the bash script within Ruby
begin
  result = %x{#{bash_script}}
  puts result  # Print the result if needed
rescue => e
  puts "Error executing bash script: #{e.message}"
end
