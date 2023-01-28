# yuzu-server-aws
File to create a dedicated server for yuzu, using aws t2.micro on us-east-#

# steps

1 - create a aws account 

# this steps are for terraform, only need to change some info in the file and run it
1.2 download the awscli and install it.

1.3 in aws browser go to IAM section, look for "Access Keys" click, and create new access key, save the file.

1.4 on cmd put aws configure and should look something like this:
        AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
        AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
        Default region name [None]: us-west-2
        Default output format [None]: json

# if you wanna run it by yourself

2 - search for EC2

2.1 launch instance, select some of the free tiers that amazon provide. use a linux aws if is possible but a ubuntu or debian instance should work as well (this probably wont run on windows).

2.2 once you have it select and go to security, on inbound rules select the security group

2.3 on the security group, select the one with the same name of the one you see in the last tab, down you will see an "Edit Inbound Rules" click it.

2.4 create one custom type for TCP and other for UDP, on port range use 24872 for both and on sources select anywhere-ipv4 then click on save rules

2.5 go to the instance, right click (if is stop click on start and wait a bit) click on connect, select the first option (EC2 instance connect) and connect.

2.6 then you will need to install docker, to run the server and docker compose
    
    sudo amazon-linux-extras install docker (this will download docker)
    
    sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
    
    sudo chmod +x /usr/local/bin/docker-compose (all this will download and give permission to -  docker compose)
    
    docker-compose version (this will check if is ok)

    sudo service docker start (this will start the docker daemon)

2.7 now, it's time to create a .yml to configure the docker image

    check the yuzu.yml on the repo, can open it with visual code or any other editor, once open,change room-name, room-description, preferred-game, preferred-game-id, max_members, token (the token should be assign once you paid for the patreon) and password, in the terminal of aws use cat > yuzu.yml, hit enter, copy and paste the info in yuzu.yml.

2.8 the copy and paste docker-compose -f yuzu.yml up -d, hit enter
    if you see a permision problem use sudo chmod 777 yuzu.yml

You will need to start the daemon using sudo service docker start and use the docker-compose to run the yml file everytime you stop the instance, is important to know that once you stop to use the instance stopped it, to prevent unexpected charges to your credit card.

With this you will setup the room, just look on yuzu, multiplayer, browse public game lobby, and your server should be there doble click it and put the password.
