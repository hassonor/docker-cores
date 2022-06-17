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