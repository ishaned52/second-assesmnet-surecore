1




share Docker socket from the host into Jenkins container     to check available images


docker run `
  --name jenkins-blueocean `
  --restart=on-failure `
  --detach `
  --network jenkins `
  --publish 8085:8080 `
  --publish 50000:50000 `
  --volume jenkins-data:/var/jenkins_home `
  --volume /var/run/docker.sock:/var/run/docker.sock `
  --user root `
  myjenkins-blueocean:2.516.1-1



  now    docker images     code will return images




  removed these lines from initial running command


  --env DOCKER_HOST=tcp://docker:2376 \
--env DOCKER_CERT_PATH=/certs/client \
--env DOCKER_TLS_VERIFY=1 \
--volume jenkins-docker-certs:/certs/client:ro \


Added this line instead:

--volume /var/run/docker.sock:/var/run/docker.sock \


Why?
Mounting the Docker socket lets Jenkins communicate with the host Docker daemon directly.

No need for TLS certs or environment variables related to TLS.

Simpler setup and fewer moving parts.


Important:
Your container needs permission to access the Docker socket.

If your container runs as user jenkins, you might need to adjust permissions on the host socket or run the container as root for Docker commands to work.

