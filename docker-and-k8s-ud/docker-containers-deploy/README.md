#### Deploying with AWS ECS: A Managed Docker Container Service - Example #1

1. On AWS Console Navigate to `Elastic Container Service`.
2. Press on `Get Started` button.
3. On Container definition section press on `Configure` on `custom`.
4. Give some Container name for e.g.: `node-demo`;
5. Insert to Image Text Area the repository name for e.g.: `orh87/node-example-1`
    1. (if you use another provider of
       DockerHub, need to include hostname and url);
    2. If it is a  `private` repo , select the checkbox `Private repository authentication` and add credentials.
6. Add the port `80` (TCP) on the Port mapping
7. Select the `Log configuration` for Auto-configure `CloudWatch` Logs
8. Press on `Update` Button
9. Press `Next` Button
10. Press `Next` Button
11. Press `Next` Button
12. Press `Create` Button`
13. After Finishing the Launching, Press `View Service`
14. Press `Tasks`
15. Press the `Task ID`
16. Copy The Public IP to Browser and Paste it.
17. We will see the App site :).

#### Updated the Previous Container

1. On my local machine:`docker built -t node-dep-example-1 .`
2. `docker tag node-dep-example-1 orh87/node-example-1`
3. `docker push orh87/node-dep-example-1`
4. On `Amazon ECS` go to `Cluster` than press `default`
5. Press on `Tasks`
6. Press on the row on `Task definition`
7. Press on `Create new revision`
8. Press on `Create` (It will pull last version of the previous image of the cluster).
9. Press on `Actions` Dropdown and select `Update Service`
10. Press on `Skip to review`
11. Press on `Update Service` Button.
12. Press on the new `Task` definition and copy the new Public IP
13. Copy it on your browser.
14. We will see the updated App site :).