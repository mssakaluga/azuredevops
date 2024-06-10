sed -i '/{ig\.document\.streaming\.default\.largeContent}/,/\<\property\>/ s,\(</property>\),\1\n<property\ name="maxDistanceAllowedForInsuranceEligibility"\ value="${ig.insurance.eligibility.distanceInMiles}" \/>,1' /eClinicalWorks/prodapps/tomcat_UAT/webapps/ihub/WEB-INF/classes/resources/ihub-context.xml



To add the `sed` command in a Ruby script, you can use the backtick syntax or the `system` method to execute shell commands. Here's how you can do it with both methods:

### Using Backticks

Backticks allow you to capture the output of the command:

```ruby
output = `sed -i '/{ig\\.document\\.streaming\\.default\\.largeContent}/,/<\\property>/ s,\\(</property>\\),\\1\\n<property\\ name="maxDistanceAllowedForInsuranceEligibility"\\ value="${ig.insurance.eligibility.distanceInMiles}" \\/>,1' /eClinicalWorks/prodapps/tomcat_UAT/webapps/ihub/WEB-INF/classes/resources/ihub-context.xml`
```

### Using `system` Method

The `system` method executes the command without capturing its output:

```ruby
system('sed -i \'/\{ig\\.document\\.streaming\\.default\\.largeContent\}/,/<\\property>/ s,\\(</property>\\),\\1\\n<property\\ name="maxDistanceAllowedForInsuranceEligibility"\\ value="${ig.insurance.eligibility.distanceInMiles}" \\/>,1\' /eClinicalWorks/prodapps/tomcat_UAT/webapps/ihub/WEB-INF/classes/resources/ihub-context.xml')
```

### Explanation

- `sed -i` is used to edit the file in place.
- The pattern `/\{ig\.document\.streaming\.default\.largeContent\}/,/\<\property\>/` specifies the range within the file to apply the substitution.
- `s,\(</property>\),\1\n<property\ name="maxDistanceAllowedForInsuranceEligibility"\ value="${ig.insurance.eligibility.distanceInMiles}" \/>,1` specifies the substitution pattern.
- The file path at the end specifies the file to be edited.

### Complete Ruby Script Example

Here is a complete example of how to integrate this `sed` command into a Ruby script:

```ruby
# ruby_script.rb

# Using backticks
output = `sed -i '/{ig\\.document\\.streaming\\.default\\.largeContent}/,/<\\property>/ s,\\(</property>\\),\\1\\n<property\\ name="maxDistanceAllowedForInsuranceEligibility"\\ value="${ig.insurance.eligibility.distanceInMiles}" \\/>,1' /eClinicalWorks/prodapps/tomcat_UAT/webapps/ihub/WEB-INF/classes/resources/ihub-context.xml`

# Or using system
system('sed -i \'/\{ig\\.document\\.streaming\\.default\\.largeContent\}/,/<\\property>/ s,\\(</property>\\),\\1\\n<property\\ name="maxDistanceAllowedForInsuranceEligibility"\\ value="${ig.insurance.eligibility.distanceInMiles}" \\/>,1\' /eClinicalWorks/prodapps/tomcat_UAT/webapps/ihub/WEB-INF/classes/resources/ihub-context.xml')

# You can also check the exit status
if $?.exitstatus != 0
  puts "An error occurred while running the sed command"
else
  puts "sed command executed successfully"
end
```

This script executes the `sed` command and checks for errors. You can use either method (`output` or `system`) based on your needs.