# aws quicksight

> CLI for AWS QuickSight.
> Access QuickSight entities.
> More information: <https://docs.aws.amazon.com/cli/latest/reference/quicksight/>.

- List datasets:

`aws quicksight list-data-sets --aws-account-id {{aws_account_id}}`

- List users:

`aws quicksight list-users --aws-account-id {{aws_account_id}} --namespace default`

- List groups:

`aws quicksight list-groups --aws-account-id {{aws_account_id}} --namespace default`

- List dashboards:

`aws quicksight list-dashboards --aws-account-id {{aws_account_id}}`

- Display detailed information about a dataset:

`aws quicksight describe-data-set --aws-account-id {{aws_account_id}} --data-set-id {{data_set_id}}`

- Display who has access to the dataset and what kind of actions they can perform on the dataset:

`aws quicksight describe-data-set-permissions --aws-account-id {{aws_account_id}} --data-set-id {{data_set_id}}`


# Custom ...........................................................................................
[Workshop Studio](https://catalog.workshops.aws/quicksight/en-US/anonymous-embedding/2-embedding-workflow)

## networking
[Connecting to a VPC with Amazon QuickSight - Amazon QuickSight](https://docs.aws.amazon.com/quicksight/latest/user/working-with-aws-vpc.html)
By creating a VPC connection in QuickSight, you're adding an elastic network interface in your VPC. This network interface allows QuickSight to exchange network traffic with a network instance within your VPC
The QuickSight network interface alone doesn't give QuickSight direct access to your databases. The VPC connection works only for the QuickSight data sources that are configured to use it.
There is only one QuickSight network interface for each VPC. It is created on the subnet that you set up in the QuickSight VPC connection.


ii. Set the session variable of the parameter to 'md5:

   set password_encryption = 'md5';

iii. Create a user and assign it the necessary credentials

   create user (username) with password '(password)';
   grant connect on database (database) to (username);
