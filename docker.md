# Docker

## Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get [http://%2Fvar%2Frun%2Fdocker.sock/v1.39/containers/json](http://%2Fvar%2Frun%2Fdocker.sock/v1.39/containers/json): dial unix /var/run/docker.sock: connect: permission denied

* gpasswd -a $USER docker
* sudo ln -s /lib/systemd/system/docker.service docker.service





## Have you had a chance to answer the previous question?

Yes, after a few months we finally found the answer. Sadly, Mike is on vacations right now so I'm afraid we are not able to provide the answer at this point.



