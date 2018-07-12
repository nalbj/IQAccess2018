BIG-IQ Partial Deployment \| Partial Restore
============================================

WORKFLOW 1: Create multiple changes. Deploy single change. (REQUIRED)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Objective
^^^^^^^^^

The user has the ability to select a specific change out of many made
for deploy.

Partial Deployment
^^^^^^^^^^^^^^^^^^

**Method 1 –** Partial Deployment selection done from Deployment
EVALUATE & DEPLOY.

-  Add an additional node to pool member.

-  ConfigurationPools: Enter “app1pool” in the upper right Filter and
   search, Select pool “app1pool” on either BOS-vBIGIP01 or 02.

-  1\ :sup:`st` change

   -  Click on New Member

|image77|

-  Select from Existing Node “app1node1” on port 80 HTTP

    |image78|

-  Click on “Save and Close” on lower right

-  2\ :sup:`nd` change – Create a New Monitor “mon-https”

   -  Click on left navigation panel “Monitors” and click on
      “Create”\ |image79|

   -  New Monitor

      -  Name: mon-https

      -  Type: HTTPS

      -  Monitor template: https

      -  username: admin

      -  password: admin and confirm password.

   -  Click “Save and Close”

|image80|

-  Add newly created Health Monitor “mon-https” to Pool “app2pool”

   -  On Pools, search app2pool

   -  On Health Monitors, select /Common/mon-https

|image81|

-  Save and Close

-  Create evaluation and deploy changes

   -  Click on top Deployment tab

   -  On left navigation panel, under EVALUATE & DEPLOY: Local Traffic &
      Network

   -  Click Create under Evaluations

|image82|

-  Name: partial-deploy

-  From Evaluation: Source Scope, Select “Partial Changes”

-  From Source Objects: Available, select “Pools”, from pool list,
   select “app1pool” for

-  BOS-vBIGIP01 & 02, and add them to Selected on the right

-  Under Target Devices, click “Find relevant devices”, select both and
   add to right

-  Click “Create” to complete

|image83|

***Note:** Only changes to “app1pool” will be deployed.*

-  Deploy changes

   -  Two methods to deploy

      ***Note**: Method 2 will not be part of this deployment.*

-  Method 1

   -  Once Evaluation is completed, click on View link to see the
      differences

|image84|

-  Cancel to dismiss the popup window and click on Deploy under
   Evaluation

-  Confirm by click on Deploy

|image85|

-  

**Method 2:** From “Configuration” tab on top, the user can select the
source object and deploy

-  Select both “app1pool” from Configuration: Pools, use filter if
   desired

-  Click “Deploy”

|image86|

-  ***Note**: This will add to the source objects list automatically for
   the evaluation task.*

|image87|

-  Partial Changes is selected and “app1pool” for both LTMs are added to
   source object list for page to create evaluation task.

-  You must use “Find relevant devices” to select the devices to move
   them to the right

-  After evaluation is finished, click on “View” to see the difference

-  Click on “Deploy now” in the Schedule area to deploy

**Note:** The deployment could fail if the targeted BIG-IP devices are
not in full sync on configurations, due to timeout on waiting for sync
to complete on target devices. Ensure the devices are in full sync
before deploying changes.

|image88|

WORKFLOW 2: Create and deploy multiple changes with selected roll-back. (REQUIRED)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Objective
^^^^^^^^^

Partial Restore – Roll Back a change
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Note: Use the two changes made in the step above.

-  Deployment: Evaluate & Deploy: Local Traffic & Network.

-  Create task using “Source Scope: All Changes”

-  Select Devices Targeted.

-  Verify all changes are part of the deployment.

   -  Add node to pool “app1pool”

   -  Add Health Monitor to “app2pool”

-  Deploy and observe completion

|image89|

-  To Rollback a change, you need to create a Partial Restore
   Evaluation.

   -  Deployment: RESTORE: Local Traffic & Network.

|image90|

-  Add name “partial-restore” and select the Snapshot created when
   deployment occurred.

   ***Note:** Duplicate names are allowed so Deployment Date is provided
   as a reference.*

-  User can narrow the scope of the restore from Full to Partial. For
   this lab let’s select Partial Restore from the Restore Scope section.

-  ***Note**: User can “Create Evaluation” or if urgent “Restore
   Immediately”.*

|image91|

-  Select “Add” for Source Objects

-  Select “/Common/app1pool” and “Add”.

-  Verify difference between BIG-IQ and Snapshot.

    |image92|

    |image93|

-  Save and Create

-  The user can restore the partial change defined from the Snapshot
   deployment.

    |image94|

|image95|

.. |image77| image:: media/image74.png
   :width: 6.50000in
   :height: 2.31250in
.. |image78| image:: media/image75.png
   :width: 6.07431in
   :height: 2.87061in
.. |image79| image:: media/image76.png
   :width: 6.22917in
   :height: 2.67708in
.. |image80| image:: media/image77.png
   :width: 6.48958in
   :height: 4.21875in
.. |image81| image:: media/image78.png
   :width: 6.50000in
   :height: 4.22917in
.. |image82| image:: media/image79.png
   :width: 5.07292in
   :height: 4.22743in
.. |image83| image:: media/image80.png
   :width: 6.50000in
   :height: 4.92361in
.. |image84| image:: media/image81.png
   :width: 6.50000in
   :height: 3.52778in
.. |image85| image:: media/image82.png
   :width: 6.50000in
   :height: 3.52778in
.. |image86| image:: media/image83.png
   :width: 6.50000in
   :height: 4.92361in
.. |image87| image:: media/image84.png
   :width: 6.50000in
   :height: 4.69792in
.. |image88| image:: media/image85.png
   :width: 6.50000in
   :height: 4.92361in
.. |image89| image:: media/image86.png
   :width: 6.50000in
   :height: 3.52778in
.. |image90| image:: media/image87.png
   :width: 4.70833in
   :height: 1.05460in
.. |image91| image:: media/image88.png
   :width: 6.50000in
   :height: 4.94792in
.. |image92| image:: media/image89.png
   :width: 4.22917in
   :height: 2.20722in
.. |image93| image:: media/image90.png
   :width: 6.50000in
   :height: 4.43750in
.. |image94| image:: media/image91.png
   :width: 6.50000in
   :height: 1.57292in
.. |image95| image:: media/image92.png
   :width: 4.18547in
   :height: 2.20833in
