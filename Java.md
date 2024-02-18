# data structure and algorithms  
https://www.geeksforgeeks.org/learn-data-structures-and-algorithms-dsa-tutorial/

# https://www.geeksforgeeks.org/user-defined-packages-in-java/

# https://www.w3schools.com/java/java_packages.asp

# https://phoenixnap.com/kb/install-maven-windows


# How to install & setup maven for java on Mac   
- https://www.baeldung.com/install-maven-on-windows-linux-mac
  -  go to https://maven.apache.org/download.cgi and download Binary tar.gz archive
  -  untar your download file: > tar -zxvf your_file_name.tar.gz
  -  mv the folder to the place as you prefered. I am using ./App within my home folder
  -  add the bin path to your enviroment PATH
    -  find your shell: > echo $SHELL
    -  I am using zsh, so I will edit .zshrc
    -  insert the line to the .zshrc:  export PATH="$PATH:/Users/huangrh/Applications/apache-maven-3.9.6/bin"
  -  on vs code, install the Extension Pack for Java
  -  then You can create a Java project using the Java: Create Java Project command. Bring up the Command Palette (⇧⌘P) and then type java to search for this command.
    -  After selecting the command, you will be prompted for the location and name of the project.
    -  You can also choose your build tool from this command. and choose maven

