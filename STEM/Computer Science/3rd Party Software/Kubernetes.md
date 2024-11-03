---
aliases:
  - K8s
---

[[3rd Party Software]]
(Info per Adam Udi's presentation)

Containerization in its modern form was made popular by Docker.
Containers are a form of OS-level virtualization. (Usually Linux-like)
Each container looks, feels, and acts like a separate OS to the programs running inside it. This is an illusion.
Unix systems use kernel features to achieve this isolation.
Every container is sharing an OS vs VMs which all have their own OS.

![[Pasted image 20241017123950.png]]

Why use Containers:
* Separation of Responsibility
* Isolation
* Portability

![[Pasted image 20241017124843.png]]

![[Pasted image 20241017124906.png]]

![[Pasted image 20241017125436.png]]
* Also it's cheaper than EKS
![[Pasted image 20241017130920.png]]
* Control Plane eg: Rancher

![[Pasted image 20241017131023.png]]

![[Pasted image 20241017131315.png]]

![[Pasted image 20241017131835.png]]

![[Pasted image 20241017132036.png]]

![[Pasted image 20241017132206.png]]

