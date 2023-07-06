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
[quicksight.pptx]($HOME/vimwiki/help/aws/quicksight.pptx)

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


### Create a QuickSight database user with the 'md5' password encryption at session level
In order to quickly allow QuickSight to connect to the RDS PostgreSQL instance, you may consider creating a dedicated QuickSight user with the compatible password encryption. To do so, connect to the RDS PostgreSQL instance using a master user or user who can create additional users and perform the following:

   i. Verify current password_encryption value:

   show password_encryption;

   ii. Set the session variable of the parameter to 'md5:

   set password_encryption = 'md5';

   iii. Create a user and assign it the necessary credentials

   create user (username) with password '(password)';
   grant connect on database (database) to (username);

 Note: add any additional permissions as required.

   iv. Use the user to connect from QuickSight and it should be able to connect successfully using the "md5" encryption and not "scram-sha-256".


## Usermanagement/Authentication
Kai Baukus
Anthony.Carnevale@bmw.de; DE-300
Aladawy Dina Khaled Saber Amin, DE-720 <Dina-Khaled-Saber-Amin.Aladawy@bmw.de>; (Dev)
Huber Florian, DE-720 <Florian.MA.Huber@bmwgroup.com>

via CDH login (AD Gruppe)
muss im CDH Cloudroom sein
Ticket zum CDH (Rollennahme und Account-ID)

CDH Core: Focus Hour
IDP muss quicksight (service-provider) requests akzeptieren
Authorisierung über ADGruppe via CDH

[CDH tutorial](https://data.bmw.cloud/docs/reference_guides/x_5_quicksight.html)
[CDH Developer](https://data.bmw.cloud/docs/for_cdh_developers/auth/index.html#quicksight-support)
[Confluence: QuickSight User management](https://atc.bmwgroup.net/confluence/display/CDHX/QuickSight+User+management)
[Log into Atlassian - ATC Confluence](https://atc.bmwgroup.net/confluence/display/CDHX/Federated+Service+Interface+Contract+TEMPLATE)

[ADGR](https://adgr-prod.bmwgroup.net/adgr/client_selection.jsf?dswid=515)

05.07.23:
1. Quicksight Prod vs Test
2. Location of Quicksight
3. Limitations
4. Parallel Use of other Auth


## SPICE
- incremental update: immer noch hinzufügen, läuft voll
- direkct query (teurer), delta load (add 1 tag delete 1 tag) lösung ist unbekannt




# Resources ........................................................................................
[How to create a data source using Terraform - #3 by larry - Question & Answer - Amazon QuickSight Community](https://community.amazonquicksight.com/t/how-to-create-a-data-source-using-terraform/11007/3)
[Terraform Registry](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/quicksight_vpc_connection)


# Scratch
physcial table map (physical mapping/data from datasource)
logical table map (joins, etc...)

template: point-in-time snapshot
