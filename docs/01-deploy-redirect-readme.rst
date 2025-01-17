Use Case 01: Redirect http to https traffic
===========================================

Prerequisites
-------------

This usecase assumes that a F5 BIG-IP instance, webservers and Ansible node are deployed. 
To deploy infrastructure in AWS users can use the `F5 Ansible Provisioner <https://github.com/f5alliances/f5_provisioner>`__

Overview of Use Case
--------------------

Challenge: When a BIG-IP has multiple Virtual IPs (VIPs) configured, it can be tedious to implement an SSL redirect from standard HTTP traffic
for each Virtual IP.

F5-LTM-HTTP-Redirect.yml playbook will create an SSL VIP then create the associative Port 80 SSL redirect for that VIP.

In this example, the playbook looks for F5_VIP_Name: ‘Use-Case-1-VIP’ as specified in the f5_vars.yaml variable file. 
Users can modify this variable file to create redirect service for any other VIP.

.. note::

   This will loop through the entire VIP list so this could take time depending on the number of VIPs within your BIG-IP

Use Case Setup
--------------

1. Login to the Ansible Host


2. Run the Ansible Playbook ‘F5-LTM-HTTP-Redirect.yml’ with the variable file ‘f5_vars.yml’ :

   .. code::

      cd ~/f5_ansible_use_cases/01-deploy-redirect
      ansible-playbook F5-LTM-HTTP-Redirect.yml -e @f5_vars.yml
   
3. Testing and Validating
   
   - Login to the BIG-IP
   - Navigate to Local traffic->Virtual server
   - Ensure there are 2 VIPs with same IP 
   
     - One listening on port 443
     - One listening on port 80
   
   - Access the VIP on port 80 you will be redirected to 443 (http://VIP:80)
   - The same webpage will also be accessible via VIP:443 (https://VIP:443)
   
   .. note::

      The IP address shown will differ from your IP address, make sure to use your own inventory file

   |
   .. image:: images/UseCase1-960.gif
   |
   
   .. note::

      While accessing the Virtual IP, your browser is presented with a certificate (clientssl cert) that is built with the BIG-IP. 
      You will therefore see an ‘unsafe’ message. Click proceed to website.