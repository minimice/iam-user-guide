# Create an IAM role using an AWS SDK<a name="example_iam_CreateRole_section"></a>

The following code examples show how to create an IAM role\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/IAM#code-examples)\. 
  

```
        /// <summary>
        /// Create a new IAM role which we can attach to a user.
        /// </summary>
        /// <param name="client">The initialized IAM client object.</param>
        /// <param name="roleName">The name of the IAM role to create.</param>
        /// <param name="rolePermissions">The permissions which the role will have.</param>
        /// <returns>A Role object representing the newly created role.</returns>
        public static async Task<Role> CreateRoleAsync(
            AmazonIdentityManagementServiceClient client,
            string roleName,
            string rolePermissions)
        {
            var request = new CreateRoleRequest
            {
                RoleName = roleName,
                AssumeRolePolicyDocument = rolePermissions,
            };

            var response = await client.CreateRoleAsync(request);

            return response.Role;
        }
```
+  For API details, see [CreateRole](https://docs.aws.amazon.com/goto/DotNetSDKV3/iam-2010-05-08/CreateRole) in *AWS SDK for \.NET API Reference*\. 

------
#### [ Go ]

**SDK for Go V2**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/gov2/iam#code-examples)\. 
  

```
	// CreateRole
	myRole, err := service.CreateRole(context.Background(), &iam.CreateRoleInput{
		RoleName:    aws.String(ExampleRoleName),
		Description: aws.String("My super awesome example role"),
		AssumeRolePolicyDocument: aws.String(`{
			"Version": "2012-10-17",
			"Statement": [
			  {
				"Effect": "Allow",
				"Principal": {
				  "Service": "ec2.amazonaws.com"
				},
				"Action": "sts:AssumeRole"
			  }
			]
		  }`),
	})

	if err != nil {
		panic("Couldn't create role: " + err.Error())
	}

	fmt.Println("☑️ Create Role")
	fmt.Printf("The new role's ARN is %s \n", *myRole.Role.Arn)
```
+  For API details, see [CreateRole](https://pkg.go.dev/github.com/aws/aws-sdk-go-v2/service/iam#Client.CreateRole) in *AWS SDK for Go API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/iam#readme)\. 
  

```
    public static String createIAMRole(IamClient iam, String rolename, String fileLocation ) throws Exception {

        try {
            JSONObject jsonObject = (JSONObject) readJsonSimpleDemo(fileLocation);
            CreateRoleRequest request = CreateRoleRequest.builder()
                    .roleName(rolename)
                    .assumeRolePolicyDocument(jsonObject.toJSONString())
                    .description("Created using the AWS SDK for Java")
                    .build();

            CreateRoleResponse response = iam.createRole(request);
            System.out.println("The ARN of the role is "+response.role().arn());

        } catch (IamException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return "";
    }

    public static Object readJsonSimpleDemo(String filename) throws Exception {
        FileReader reader = new FileReader(filename);
        JSONParser jsonParser = new JSONParser();
        return jsonParser.parse(reader);
    }
```
+  For API details, see [CreateRole](https://docs.aws.amazon.com/goto/SdkForJavaV2/iam-2010-05-08/CreateRole) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ JavaScript ]

**SDK for JavaScript V3**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Create the role\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { CreateRoleCommand } from "@aws-sdk/client-iam";

// Sample assume role policy JSON.
const role_json = {
    Version: "2012-10-17",
    Statement: [
        {
            Effect: "Allow",
            Principal: {
                AWS: "USER_ARN", // The ARN of the user.
            },
            Action: "sts:AssumeRole",
        },
    ],
};
// Stringify the assume role policy JSON.
const myJson = JSON.stringify(role_json);

// Set the parameters.
const params = {
    AssumeRolePolicyDocument: myJson,
    Path: "/",
    RoleName: "ROLE_NAME"
};

const run = async () => {
    try {
        const data = await iamClient.send(new CreateRoleCommand(params));
        console.log("Success. Role created. Role Arn: ", data.Role.RoleName);
        } catch (err) {
            console.log("Error", err);
        }
};
run();
```
+  For API details, see [CreateRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createrolecommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

$assumeRolePolicyDocument = "{
                \"Version\": \"2012-10-17\",
                \"Statement\": [{
                    \"Effect\": \"Allow\",
                    \"Principal\": {\"AWS\": \"{$user['Arn']}\"},
                    \"Action\": \"sts:AssumeRole\"
                }]
            }";
$assumeRoleRole = $service->createRole("iam_demo_role_$uuid", $assumeRolePolicyDocument);
echo "Created role: {$assumeRoleRole['RoleName']}\n";

    /**
     * @param string $roleName
     * @param string $rolePolicyDocument
     * @return array
     * @throws AwsException
     */
    public function createRole(string $roleName, string $rolePolicyDocument)
    {
        $result = $this->customWaiter(function () use ($roleName, $rolePolicyDocument) {
            return $this->iamClient->createRole([
                'AssumeRolePolicyDocument' => $rolePolicyDocument,
                'RoleName' => $roleName,
            ]);
        });
        return $result['Role'];
    }
```
+  For API details, see [CreateRole](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreateRole) in *AWS SDK for PHP API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/iam/iam_basics#code-examples)\. 
  

```
def create_role(role_name, allowed_services):
    """
    Creates a role that lets a list of specified services assume the role.

    :param role_name: The name of the role.
    :param allowed_services: The services that can assume the role.
    :return: The newly created role.
    """
    trust_policy = {
        'Version': '2012-10-17',
        'Statement': [{
                'Effect': 'Allow',
                'Principal': {'Service': service},
                'Action': 'sts:AssumeRole'
            } for service in allowed_services
        ]
    }

    try:
        role = iam.create_role(
            RoleName=role_name,
            AssumeRolePolicyDocument=json.dumps(trust_policy))
        logger.info("Created role %s.", role.name)
    except ClientError:
        logger.exception("Couldn't create role %s.", role_name)
        raise
    else:
        return role
```
+  For API details, see [CreateRole](https://docs.aws.amazon.com/goto/boto3/iam-2010-05-08/CreateRole) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------
#### [ Ruby ]

**SDK for Ruby**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/ruby/example_code/iam#code-examples)\. 
  

```
  # Creates a role that can be assumed by a user.
  #
  # @param role_name [String] The name to give the role.
  # @param user [Aws::IAM::User] The user who is granted permission to assume the role.
  # @return [Aws::IAM::Role] The newly created role.
  def create_role(role_name, user)
    role = @iam_resource.create_role(
      role_name: role_name,
      assume_role_policy_document: {
        Version: "2012-10-17",
        Statement: [{
          Effect: "Allow",
          Principal: {'AWS': user.arn},
          Action: "sts:AssumeRole"
        }]
      }.to_json)
    puts("Created role #{role.name}.")
  rescue Aws::Errors::ServiceError => e
    puts("Couldn't create a role for the demo. Here's why: ")
    puts("\t#{e.code}: #{e.message}")
    raise
  else
    role
  end
```
+  For API details, see [CreateRole](https://docs.aws.amazon.com/goto/SdkForRubyV3/iam-2010-05-08/CreateRole) in *AWS SDK for Ruby API Reference*\. 

------
#### [ Rust ]

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/iam#code-examples)\. 
  

```
pub async fn create_role(
    client: &iamClient,
    role_name: &str,
    role_policy_document: &str,
) -> Result<Role, iamError> {
    let response: CreateRoleOutput = loop {
        if let Ok(response) = client
            .create_role()
            .role_name(role_name)
            .assume_role_policy_document(role_policy_document)
            .send()
            .await
        {
            break response;
        }
    };

    Ok(response.role.unwrap())
}
```
+  For API details, see [CreateRole](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using IAM with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.