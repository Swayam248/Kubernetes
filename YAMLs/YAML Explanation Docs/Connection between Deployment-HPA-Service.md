**Deployment file**

<img width="405" height="451" alt="image" src="https://github.com/user-attachments/assets/e233dead-6b74-43ae-a5f9-c1c49f43df3d" />

**HPA file**

<img width="312" height="404" alt="image" src="https://github.com/user-attachments/assets/6a0e2ce0-3c67-4f8b-a87e-72bcf9db3fc2" />

**Service file**

<img width="311" height="512" alt="image" src="https://github.com/user-attachments/assets/81bafc23-45f6-47db-b1f1-3a7d51a0a8a6" />

 
## **Explanation:**

**Connection between Deployment file and HPA file.** 

HPA will know regarding the Deployment file using _scaleTargetRef_.

HPA.scaleTargetRef.name  ==  Deployment.metadata.name

---------------------------------
**Connection between Deployment file and Service file.**

Service will connect the both files  using _labels_(Deployment file) + _selector_(Service file). 

Service.selector  == Deployment.labels




