# How to backup/restore data on the same Transformation Advisor version
1. Create a copy of the folder containing the data
1. Put the contents of the old data folder into new data folder

# How to backup/restore data on Transformation Advisor upgrade/reinstall
1. Create a copy of the folder containing the data
1. Reinstall or upgrade Transformation Advisor
1. Put the contents of the old data folder into new data folder
1. Restart Server Container

# How to locate the data
### Dynamic provisioning (GlusterFS)
```bash
kubectl describe pv <pv-name>
``` 
Use **Source:Path** value to locate the data on the disk where it is stored.   

### NFS PV
```bash
kubectl describe pv <pv-name>
``` 
Use **Source:Path** value to locate the data on the disk where it is stored.   
Default in the Readme is set to **/opt/couchdb/data** and can be reset while NFS PV is created.

### Hostpath PV
```bash
kubectl describe pv <pv-name>
``` 
Use **Source:Path** value to locate the data on the disk where it is stored.   
Default in the Readme is set to **/usr/data** and can be changed on PV creation using **hostPath/path** field in the PV definition.  

### Disabled persistence
Data goes into **/opt/couchdb/data** folder in the CouchDB pod.  
```bash
kubectl -n ta exec -it <chouchdb-pod-name> bash
``` 
Locate the data folder and export its contents outside of the container.
