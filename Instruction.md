Prerequisites:
1. You are in ap-southeast-2 region, otherwise, the template will throw errors as it cannot find any subnet meet requirements.
2. You have created an IAM role with provided privileges (rolepolicy.json) for cloud formation to create all the resources.
   Here is the steps on creating the required role -
   IAM -> Roles -> Create role: trusted entity: AWS service, use case: cloudformation, then click on create policy and paste the contents in JSON tab.
   Last, attach the created policy to the new role.

Create Stack:
1. Create stack using the following S3 URL:
   https://webapp-theresa.s3-ap-southeast-2.amazonaws.com/main.yaml
   <img width="1393" alt="Screen Shot 2020-06-17 at 4 30 10 pm" src="https://user-images.githubusercontent.com/66954944/84863170-e2468e80-b0b7-11ea-8eb9-287d060bbe38.png">

2. Specify stack name
   <img width="1267" alt="Screen Shot 2020-06-17 at 2 43 08 pm" src="https://user-images.githubusercontent.com/66954944/84863551-69940200-b0b8-11ea-8599-df6b360d78c9.png">

3. Specify the role created using rolepolicy.json
   <img width="1247" alt="Screen Shot 2020-06-17 at 2 43 47 pm" src="https://user-images.githubusercontent.com/66954944/84863616-8defde80-b0b8-11ea-9cee-5614f191dbc3.png">

4. Click Next, and then Create Stack.
   This kicks off the stack provision.

5. When the stack creation finishes, click on the CDN nested stack and check the output
   <img width="1266" alt="Screen Shot 2020-06-17 at 3 36 25 pm" src="https://user-images.githubusercontent.com/66954944/84863713-bd9ee680-b0b8-11ea-949a-65d933d82598.png">

6. Click the URL and you should be able to see the “Hello World” web page using HTTPS.
   <img width="1320" alt="Screen Shot 2020-06-17 at 3 38 00 pm" src="https://user-images.githubusercontent.com/66954944/84863766-da3b1e80-b0b8-11ea-92e6-a76258fda78e.png">
