# Udacity Cloud Devops Engineer Nanodegree Capstone Project

## Project Overview

<p> This project entails the deployment of kubernetes clusters to perform a rolling deployment on AWS using jenkin CI, Cloudformation, docker images and Elastic Kubernetes Store </p>

***

<p>I developed a CI/CD pipeline for microservices applications with rolling deployment. I developed Continuous Integration steps, such as typographical checking (aka “linting”). I also developed Contiguous Deployment steps. These includes:</p>

<ul>
	<li>Pushing the built Docker containers to the Docker repository</li>
	<li>Deploying these Docker containers to a Kubernetes cluster. For Kubernetes cluster I used AWS EKS. To deploy my Kubernetes cluster using Cloudformation. I ran these from Jenkins as an independent pipeline.</li>
</ul>

***

<h2>Environment Setup</h2>

<ul>
  <li>Create a <code>Dockerfile</code></li>
  <li>Create a <code>Jenkinsfile</code> including all the necessary steps</li>
  <li>Create CloudFormation Script using <code>setup_cluster.sh</code> command</li>
  <li>Install Jenkins and all the necessary plugins in the EC2 Instance</li>
</ul>

<h2>Running App</h2>

<ul>
  <li>Run Docker: <code>./run_docker.sh</code></li>
  <li>Run Kubernetes: <code>./run_kubernetes.sh</code></li>
</ul>

<h2>Uploading to the Docker Hub</h2>

<ul>
  <li>Run <code>./upload_docker.sh</code> to upload the api to the Docker Hub</li>
</ul>

<h2>Deploying App to AWS</h2>

<pre>
	<code>
  		aws eks --region eu-central-1 update-kubeconfig --name ucdncapstonecluster
  		kubectl apply -f deployment/rolling.yaml
  		kubectl get nodes
  		kubectl get pods -o wide
	</code>
</pre>

