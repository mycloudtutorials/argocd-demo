Create Docker Image 

docker build -t mycloudtutorials/argocd-demo:1.0 .

Push Docker Image

docker push mycloudtutorials/argocd-demo:1.0

Create Another one after changing the index.php

Create Docker Image 

docker build -t mycloudtutorials/argocd-demo:2.0 .

Push Docker Image

docker push mycloudtutorials/argocd-demo:2.0
