[root@test3] # docker run -itd --name mongo -p 27017:27017 mongo --auth

    Error response from daemon: Conflict. The container name "/mysql-server" is already in use by container bdc8d8c475cb86695c466d23fd7102221f2c040898c2d576f94cd06c93ca811b. You have to remove (or rename) that container to be able to reuse that name..See '/usr/bin/docker-current run --help'.

    1.docker ps
    2.docker ps -l
    3.docker rm bdc8d8c475cb

https://blog.csdn.net/972301/article/details/80915127
https://thispointer.com/docker-how-to-stop-remove-a-running-container-by-id-or-name/

