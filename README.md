<!-- Q.1 Automate Form Filling Using Selenium and Python.       -->
<!-- create a form_fill.py -->
code

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import time
# Initialize Chrome driver
driver = webdriver.Chrome()
# Step 1: Open form page
driver.get("https://www.w3schools.com/html/html_forms.asp")
driver.maximize_window()
# Step 2: Find input fields by name
fname = driver.find_element(By.ID, "fname")
lname = driver.find_element(By.ID, "lname")
# Step 3: Clear and fill the form fields
fname.clear()
fname.send_keys("Sujata")
lname.clear()
lname.send_keys("Oak")
# Step 4: Scroll and click Submit button
driver.find_element(By.XPATH, "//input[@type='submit']").click()
# Step 5: Wait and print the new URL
time.sleep(5)
print("Form submitted successfully!")
print("Redirected URL:", driver.current_url)
# Step 6: Close browser
driver.quit()

<!-- install selenium-->
pip install selenium 
<!-- download chrome driver after cross verifying chrome version -->
link
<!-- https://googlechromelabs.github.io/chrome-for-testing/#stable -->
now place the chromedriver.exe in the same folder where your form_fill.py file is located
run the file

<!-- Q.2 Demonstrate how to deploy “Hello World” java application using a Dockerfile?    -->
install docker if not present
sudo apt update -y
sudo apt install docker.io -y
systemctl start docker
systemctl enable docker 
sudo docker --version(to verify whether installed)

<!-- first create a folder and get into it -->
mkdir java-docker-demo
cd java-docker-demo
<!-- Create a file named Hello.java -->
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, World from Docker!");
    }
}
<!-- create a file named Dockerfile -->
copy these things in that file(Dockerfile) and save in the same folder as java-docker-demo
#note no extension for Dockerfile

# Step 1: Use an official OpenJDK base image
FROM openjdk:17

# Step 2: Set working directory inside container
WORKDIR /app

# Step 3: Copy the Java file into the container
COPY Hello.java /app

# Step 4: Compile the Java program
RUN javac Hello.java

# Step 5: Command to run the program
CMD ["java", "Hello"]

<!-- Build the Docker image
In the same folder (where Dockerfile and Hello.java exist): -->
docker build -t java-hello . <!--here the t is used to define the tag so java-hello is the tag given to the image -->
docker run java-hello
<!-- to verify -->
docker images
docker ps -a

<!-- Deploy a Web Application Using Ansible playbook? -->

open aws
create instances as usual click all checks create new key pair 
create a folder(yourname) on desktop and copy this key pair in that folder
come back to instances on aws
click on instance id of anyone instance(master)
click on connect(top option next to instance state)
get in the ssh client tab
copy the link given in example
which will be something like this
(ssh -i "brick.pem" ubuntu@ec2-3-87-248-131.compute-1.amazonaws.com)

now go the terminal of linux(yoursystem/computer)
get into your folder(yourname) which you created on Desktop : cd Desktop : cd folder(yourname)
take privileges : sudo su
paste the link in the terminal : ssh -i "brick.pem" ubuntu@ec2-3-87-248-131.compute-1.amazonaws.com
type yes if asked 
done 

master is now setup on your computer 
#note the ip of master in terminal so as to not get confused between master and slave

similarly setup slave on your terminal

Ansible-master : Ansible Installation 
execute these commands in master
(sudo if not earlier taken the privilege or else no need)
# sudo apt update -y
# sudo apt-add-repository ppa:ansible/ansible
# sudo apt update -y 
# sudo apt-get install ansible -y
# ansible --version

Ansible-slave : 
# apt update -y

Ansible-master:  
# nano /etc/ansible/hosts 
add these 2 lines in the file and save it 
(
[client_1]             
172.18.17.47              #ip of the master machine
)
Ansible-master:
# ssh-keygen -t rsa 
press enter all the way until you see key's randomart image

# cd /root/.ssh/
# ls 
# cat id_rsa.pub

Copy this key into ansible-slave machine
Ansible-slave:
# cd /root/.ssh/
# ls 
# nano authorized_keys
paste here: you get this in ansible master 
ssh-rsa 
AAAAB3NzaC1yc2EAAAADAQABAAABgQCQyPV+pZYBkdvXnjPaZCrF8meausJtDLKc/8P0gZew60tBd kOFrL22yScMJmbEtqcmOP5xLIHZi0asDGtaQ9N50xp3A2AyD5oenf6yZAta1dN7qRZKslFSW4qwND+wI
 HUcRJMCwaTVPJfV0qiipd9akq0QnoGCDA/iypeeHs3e0hlK2nKZlElodxWOV+nfpowR3TXYvEl3g6Y92c
 UQWdblTM1RzQUI+qx+2MnU73evQ/HIyCsuOxFzQpzJkhqxNloP+oAZKA6mSRXfaNVSuuMiqjtO9LAh
 mw86CCjA39hJ9OIK5S21qPEmBPZnOq2Km0jVDdHa4IWJB07b8lZFU1wdi7k5PJ9U0ZrMP6FzD1uTUjs
 wyHAF7C6t/axcez+eF6RqK4hz2a8TeH2epnRNvgmVeAOxjJ94EinPPpXm2NAjHcZhxv3m4RyzAx5/teJE8
 G35vBzHDq104VkwO4Y+lQTT9c+eAZEHxoKMJYiqijW5HggepXdNFlXZuuy4MftQhZk= root@ip-172
31-16-120   (u get this is ansible master after you put in this command cat id_rsa.pub) 

Save it the nano authorized_keys

next step in slave
# nano /etc/ssh/sshd_config
add this in the config file after PermitRootLogin prohibit-password:
(
PermitRootLogin yes 
)

save it

Ansible-master:
# ansible -m ping all

These steps are not necessary just addded for safety
Ansible-slave: 
# git --version
# apt remove git

# git --version
So now I want to install git
so do it on master machine
Ansible-master:
# ansible client_1 -m apt -a "name=git state=present" --become 
Ansible-slave:
# git --version

How to uninstall package from a ansible-master machine?
Ansible-slave machine :
# nano test.txt

Ansible-master machine:
# ansible client_1 -m apt -a "name=nano state=absent" --become
Ansible-slave machine: 
# nano test.txt

Ansible-master machine:
# ansible client_1 -m apt -a "name=nano state=present" --become

Ansible-master machine:
# nano test.txt

so installation is done 
now begins 
q.3 
Step 1: Clone ansible code from my(ma'am) github repository
Ansible-master: 
# cd (to get out of /.ssh)
# ls
# mkdir ansible-lab 
# cd ansible-lab
# git clone https://github.com/sujataoak799/ansible-codes.git 
# ls 
# cd ansible-codes
# ls 
STEP2:  
Now all my files are in ansible-master machine and I need to deploy it on my ansible-slave 
machine. So we will be configuring our ansible-slave machine to host our full stack 
application.

The first playbook which I am going to setup on ansible-slave machine is lampstack_1.yml

Ansible master: 
# nano lampstack_1.yml
save it 

STEP3:  How to Run/Execute a playbook.
Ansible master: 
# ansible-playbook lampstack_1.yml 

Ansible-slave: 
get out of ./ssh

# mysql
# php --version 
# service apache2 status

Once apache service status is active. Copy IPv4 address of ansible-slave machine in browser and you can see the 
deployment of index.html page. 
the address here is 3.94.110.212 

for the change of name i.e. Anchita Patekar to be reflected here on the website 3.94.110.212 you need to edit the 
index.html file on git and commit the changes.

done complete 

<!-- Q.4 Automate YouTube Search Using Selenium and Python  -->

same as q.1
install selenium 
install chromedriver
https://googlechromelabs.github.io/chrome-for-testing/#stable
chrome driver.exe in the same folder

code

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import time

# Initialize Chrome driver
driver = webdriver.Chrome()

try:
    # Step 1: Open YouTube
    driver.get("https://www.youtube.com")
    driver.maximize_window()
    print("✅ YouTube opened successfully!")
    
    # Wait for page to load
    time.sleep(3)
    
    # Step 2: Find the search box
    # YouTube search box can be located by name, ID, or XPath
    search_box = driver.find_element(By.NAME, "search_query")
    # Alternative selectors if the above doesn't work:
    # search_box = driver.find_element(By.XPATH, "//input[@id='search']")
    # search_box = driver.find_element(By.CSS_SELECTOR, "input#search")
    
    # Step 3: Clear and enter search query
    search_box.clear()
    search_query = "Selenium WebDriver tutorial"
    search_box.send_keys(search_query)
    print(f"✅ Search query entered: '{search_query}'")
    
    # Step 4: Submit search (multiple ways)
    # Method 1: Press Enter key
    search_box.send_keys(Keys.RETURN)
    
    # Method 2: Click search button (alternative)
    # search_button = driver.find_element(By.ID, "search-icon-legacy")
    # search_button.click()
    
    print("✅ Search submitted!")
    
    # Step 5: Wait for search results to load
    time.sleep(5)
    
    # Step 6: Get and print search results information
    print("\n📊 SEARCH RESULTS INFORMATION:")
    print("Search URL:", driver.current_url)
    print("Page Title:", driver.title)
    
    # Step 7: Extract and display first few video results
    print("\n🎬 TOP VIDEO RESULTS:")
    
    # Find video elements (multiple selectors for robustness)
    video_titles = driver.find_elements(By.CSS_SELECTOR, "ytd-video-renderer #video-title")
    # Alternative selectors:
    # video_titles = driver.find_elements(By.XPATH, "//a[@id='video-title']")
    # video_titles = driver.find_elements(By.CSS_SELECTOR, "h3 a#video-title")
    
    # Display first 5 video results
    for i, title in enumerate(video_titles[:5], 1):
        video_title = title.get_attribute("title") or title.text
        if video_title:  # Only print if title exists
            print(f"{i}. {video_title}")
    
    # Step 8: Optional - Click on first video
    if video_titles:
        first_video = video_titles[0]
        first_video.click()
        print(f"\n✅ Playing first video: '{video_titles[0].get_attribute('title')}'")
        
        # Wait for video to load
        time.sleep(10)
        
        # Get video player information
        print("\n📺 VIDEO PLAYER INFO:")
        print("Current URL:", driver.current_url)
        print("Video Title:", driver.title)
        
        # Check if video is playing (basic check)
        video_player = driver.find_element(By.TAG_NAME, "video")
        is_playing = driver.execute_script("return arguments[0].currentTime > 0", video_player)
        print("Video playing:", "Yes" if is_playing else "No")
    
    # Wait to observe
    time.sleep(5)

except Exception as e:
    print(f"❌ An error occurred: {e}")
    
finally:
    # Step 9: Close browser
    driver.quit()
    print("🔒 Browser closed.")


<!-- Q.5 Demonstrate how to deploy “factorial of a number” java application using a Dockerfile? -->
similar to Q.2
install docker if not present
sudo apt update -y
sudo apt install docker.io -y
systemctl start docker
systemctl enable docker 
sudo docker --version(to verify whether installed)

mkdir factorial-docker-demo
cd factorial-docker-demo

create a Factorial.java file in the factorial-docker-demo folder

code
import java.util.Scanner;

public class Factorial {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);  // Create a Scanner object for input
        System.out.print("Enter a number: ");
        int num = sc.nextInt();  // Take integer input from user

        int fact = 1;

        // Loop to calculate factorial
        for (int i = 1; i <= num; i++) {
            fact = fact * i;
        }

        System.out.println("Factorial of " + num + " is: " + fact);
    }
}

create a file named Dockerfile in the same folder(factorial-docker-demo)
add this in Dockerfile(no extension)
# Use official OpenJDK runtime as base image
FROM openjdk:17-jdk-slim

# Set the working directory in the container
WORKDIR /app

# Copy the Java source file to the working directory
COPY Factorial.java .

# Compile the Java program
RUN javac Factorial.java

# Run the application in interactive mode
CMD ["java", "Factorial"]

commands to be given in the terminal

# docker build -t factorial-app
# docker run -it factorial-app or docker run -i factorial-app
 
done
to verify
# docker images
# docker ps -a

<!-- Q.6 Automate Wikipedia Search and Extract Page Title -->

similar to q.1

install selenium
chromedriver if not installed
https://googlechromelabs.github.io/chrome-for-testing/#stable
chrome driver.exe in the same folder

create a file named wikipedia.py
code

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

class WikipediaAutomation:
    def __init__(self):
        self.driver = webdriver.Chrome()
        self.wait = WebDriverWait(self.driver, 10)
    
    def open_wikipedia(self):
        """Open Wikipedia homepage"""
        print("🌐 Opening Wikipedia...")
        self.driver.get("https://www.wikipedia.org")
        self.driver.maximize_window()
        
        # Wait for page to load completely
        self.wait.until(EC.presence_of_element_located((By.ID, "searchInput")))
        print("✅ Wikipedia loaded successfully!")
    
    def search_and_extract(self, search_query):
        """Search for a term and extract page title"""
        print(f"\n🔍 Searching for: '{search_query}'")
        
        try:
            # Find search box and enter query
            search_box = self.driver.find_element(By.ID, "searchInput")
            search_box.clear()
            search_box.send_keys(search_query)
            search_box.send_keys(Keys.RETURN)
            
            # Wait for search results to load
            self.wait.until(EC.presence_of_element_located((By.ID, "firstHeading")))
            
            # Extract page title
            page_title = self.driver.find_element(By.ID, "firstHeading").text
            print(f"✅ PAGE TITLE: '{page_title}'")
            
            # Extract additional information
            self.extract_page_info()
            
            return page_title
            
        except Exception as e:
            print(f"❌ Error during search: {e}")
            return None
    
    def extract_page_info(self):
        """Extract additional page information"""
        try:
            print("\n📊 DETAILED PAGE INFORMATION:")
            print("📍 Page URL:", self.driver.current_url)
            print("🏷️  Browser Title:", self.driver.title)
            
            # Extract first paragraph
            first_para = self.driver.find_element(By.CSS_SELECTOR, "div.mw-parser-output > p")
            para_text = first_para.text[:300] + "..." if len(first_para.text) > 300 else first_para.text
            print(f"📝 Summary: {para_text}")
            
            # Extract infobox details if available
            try:
                infobox = self.driver.find_element(By.CLASS_NAME, "infobox")
                print("📋 Infobox: Available")
            except:
                print("📋 Infobox: Not available")
                
        except Exception as e:
            print(f"⚠️ Could not extract complete page info: {e}")
    
    def search_multiple_topics(self, topics):
        """Search multiple topics sequentially"""
        for topic in topics:
            # Go back to Wikipedia homepage for new search
            self.driver.get("https://www.wikipedia.org")
            time.sleep(2)
            
            # Perform search and extraction
            self.search_and_extract(topic)
            
            # Wait before next search
            time.sleep(3)
    
    def close(self):
        """Close the browser"""
        self.driver.quit()
        print("🔒 Browser closed.")

# Main execution
if __name__ == "__main__":
    wiki_bot = WikipediaAutomation()
    
    try:
        # Open Wikipedia
        wiki_bot.open_wikipedia()
        
        # Search for multiple topics
        search_topics = [
            "Artificial Intelligence",
            "Machine Learning", 
            "Python Programming"
           
        ]
        
        print("=" * 60)
        print("🚀 WIKIPEDIA SEARCH AUTOMATION")
        print("=" * 60)
        
        wiki_bot.search_multiple_topics(search_topics)
        
        print("\n" + "=" * 60)
        print("✅ ALL SEARCHES COMPLETED SUCCESSFULLY!")
        print("=" * 60)
    
    except Exception as e:
        print(f"❌ Error: {e}")
    
    finally:
        # Close browser
        wiki_bot.close()


<!-- Q.7 Demonstrate how to deploy “addition of a number” java application using a Dockerfile?-->

mkdir addition-docker
cd addition-docker

Addition.java
import java.util.Scanner;

public class Addition {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("=== ADDITION CALCULATOR ===");
        
        // Get first number
        System.out.print("Enter first number: ");
        int num1 = scanner.nextInt();
        
        // Get second number
        System.out.print("Enter second number: ");
        int num2 = scanner.nextInt();
        
        // Calculate sum
        int sum = num1 + num2;
        
        // Display result
        System.out.println("Result: " + num1 + " + " + num2 + " = " + sum);
        
        scanner.close();
    }
}

Dockerfile

# Use OpenJDK base image
FROM openjdk:17-jdk-slim

# Set working directory
WORKDIR /app

# Copy Java file
COPY Addition.java .

# Compile Java program
RUN javac Addition.java

# Run the application
CMD ["java", "Addition"]


docker build -t addition-app .
docker run -it addition-app or docker run -i addition-app

to verify 
docker images
docker ps -a

<!-- Q.8 Demonstrate in EC2 Instance the installation of Jenkins? -->
go to aws
launch an instance 
click on instance id
connect to the terminal on ec2 instance

commands on ec2 instance terminal or system terminal
# sudo su
# apt-get update
# apt-get install openjdk-11-jdk
# java -version
# curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
# echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
# sudo apt-get update
# sudo apt-get install jenkins
# jenkins --version
# sudo systemctl start jenkins.service or sudo systemctl start jenkins
# sudo systemctl status jenkins
adjust firewall and configure jenkins
# sudo ufw status
# sudo ufw allow 8080
# sudo ufw enable 
# sudo ufw status

Once installation is done, you can test the application on http://localhost:8080
inbrowser OR http://127.0.0.1:8080

you will see a screen asking for password
terminal
# sudo cat /var/lib/jenkins/secrets/initialAdminPassword
paste the text after this command in browser
install suggested plugins

<!-- Q.9 Create and run a job in Jenkins for simple HelloWorld in Java using a version control 
tool -->

Step 1: Go to the Jenkins dashboard and click on the New Item.
Step 2: In the next page, enter the item name, and select the 'Freestyle project' option. And click
OK. Here, my item name is HelloWorld.
Step 2: In the next page, enter the item name, and select the 'Freestyle project' option. And click
OK. Here, my item name is HelloWorld.
Step 3: When you enter the OK, you will get a configuration page. Enter the details of the
project in the Description section.
Description: Author: XYZ
Create and run a job in Jenkins for simple Helloworld in java
Step 4: On the Source Code Management section, select the Git option, and specify the
Repository URL.

To do that you should have proper github setup on your system. To do the github setup:
First, you have to create a project in java. Here, I created a simple HelloWorld program
and saved it to one folder i.e. Desktop/JENKINS_LAB. Compile the HelloWorld.java file.

system terminal
cd Desktop
mkdir JENKINS_LAB
gedit Simple.java
javac Simple.java
java Simple
Now create a project in your GitHub account and give the Repository name. Here my
repository name is HelloWorld_29072024.
(public 
tick add a readme file)
Your repository is created. Copy the repository URL. My repository URL is:
https://github.com/sujataoak799/HelloWorld_29072024.git
o Open the command prompt in your Ubuntu and go to the path where your java file is
created.
o Then run the following command.
git init
git status
git add .
git status
Configure your GitHub account in your system.

1. git config --global user.email "your@email"
2. git config --global user.name "username"
git config --list (to verify)
Commit it and add the repository URL.
1. git commit -m "Added HelloWorld Java Program"
2. git remote add origin https://github.com/sujataoak799/HelloWorld_29072024.git
3. git push -u origin master
Now, when you refresh your GitHub account, the HelloWorld file will be added in your
repository.
Step 5: Add the Repository URL in the Source Code Management section.(https://github.com/sujataoak799/HelloWorld_29072024.git) #this url
Step 6: Now, it is time to build the code. Click on "Add build step" and select the "Execute
Shell".
Step 7: Enter the following command to compile the java code.
javac Simple.java
java Simple
Step 8: Click Apply and then Save button.
Step 9: Once you saved the configuration, then now can click on Build Now option.
Step 10: After clicking on Build Now, you can see the status of the build on the Build History
section.
Once the build is completed, a status of the build will show if the build was successful or not. If
the build is failed then it will show in red color. Blue symbol is for success.
Click on the build number #2 in the Build History section to see the details of the build.
Step 11: Click on Console Output from the left side of the screen to see the status of the build
you run. It should show the success message.

<!-- Q.10  Implement Jenkins Master-Slave Architecture with Scaling  -->

STEP1: In Jenkins Dashboard Click on Manage Jenkins -> Manage Nodes or just nodes
STEP 2: Select New Node and enter the name of the node in the Node Name field.
Select Permanent Agent and click the OK button. Initially, you will get only one option,
“Permanent Agent.” Once we have one or more slaves you will get the “Copy Existing Node”
option. Click Create
STEP3: Configure node with below details:

system terminal
create a folder on Desktop JENKINS_LAB
# pwd
this gives the path which you gonna need later on
# sudo su

# find / -type f -name java or find /usr /opt -type f -name java 2>/dev/null
the path u get after this command for jdk u need it so save it

now jenkins shows node setup (description and all)
node name: agent2
Description: This is a demo on master slave jenkins
no. of executors 1
remote directory: pwd path
usage: only build jobs
launch method: launch agent by connecting to the controller
availability: keep this agent online as much as possible

Under ‘Node Properties’, provide jdk path.
tick environment variables
add
name: java_home
value: path u get after this command above (find / -type f -name java) 
save

STEP4: On click of ‘Save’ will display the below page with error message. Here Jenkins connect
with Slave node using Java Web Start and it needs a port to establish the connection.
To configure JNLP port in global security. Now goto Manage Jenkins -> Security

in security go to agents 
fixed: 5555

system terminal
# sudo ufw allow 5555/tcp
STEP5: Again coming back to Jenkins and navigate to Nodes -> agent2 which will display two
ways to connect with Agent node.

run from agent command line (unix):
run all the commands of unix on terminal

STEP 6: Create a New Job in Jenkins dashboard

name of new item: master_slave_jenkins_demo
freestyle project
ok
STEP 7: Configure the page with following:

Description: Demo on jenkins master slave architecture
tick restrict where project can be run 
label expression
agent2

Build steps
execute shell
echo "Hello , welcome to session on MASTER SLAVE ARCHITURE."
apply and save

Click on Build-Now, Console Output

STEP 8: Goto Jenkins Dashboard->Manage Jenkins->Nodes->agent2
final output 