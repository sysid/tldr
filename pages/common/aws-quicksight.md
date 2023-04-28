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
Event-hash: de0a-152502b6b4-09
Account 311036604259
User: TeamRole/MasterKey

[Workshop Studio](https://catalog.workshops.aws/quicksight/en-US/author-workshop)
https://catalog.workshops.aws/quicksight/en-US/author-workshop

QuicksightRoom: training4tw


https://dashboard.eventengine.run/dashboard

Login:
https://signin.aws.amazon.com/federation?Action=login&Issuer=https%3A%2F%2Fdashboard.eventengine.run&Destination=https%3A%2F%2Fus-east-1.console.aws.amazon.com%2Fconsole%2Fhome&SigninToken=t3_4LQpaJrvm0AE6QOWX_RmljqushqZbvw_qYb5foEqf8u9BfIrjetySRqtwhU8PpbT1yvhXfIH0wPxX5l0sheHo0vaV0zsPQBh5NOfCnECtU3A8offA8U9T7n3szeDikmwIp0ahex64Yu8pUqXo-s24LGDYtfXvhYeF9HZxsqy7pNJLeKQyEOPS-OGH9sWfpTlGfEHXfVJPKcBMNiF71ZrsDJWPx3TryrQNh7ChaeQCyQR5U-5B656VOqmtKINt6TJAZ4SPEss370g5axJ1Ihmg7EfsFV_U11pQ4ITvS4vNg05ETXUHpHH1nvDu-vDdUq7vhdXI6jgJomSbTV64XGBUah8hAH0DXOpLPatWCcZp_ydQlwD9709Wpciqms4wraDpwW515L81KEJmDwvWGXyqY5wV8UxLWn4AkfCqjhmFwwToDRYWy1ThpHyx3qjv4BTh8v6v4bLWMP8Fl7A__Hp4yO8dRLlOGr8Mn8wFa2q6PuO-Ab33hueSgu-M7Gy3GA-220puFmNUJ4KTCEoWCxFgqpG5yD2FLWWxAnWsDRv0FvqX7g2JEO9aK1-GcIT-vwbwK0jaebCVQGWeHPLRA-d7tiOhs9RoiELsYc9i5AprhWosIhwN2hRfatDnUAi0spqRnIo5lzU9SDJNj5Zpgwd5SaTCExwww0M-Znf-7EoXb-a7gp2jJ9umzieoL7kgTFCUCfrs_pqNavp-MR2eS-tJBF197HPycxqC08_FdWIFt8pTHkvaxadiP28gp1YHSHnl48j-8PJgZCr2DEswyYt2N0ZBG1RGW7PL_y8hQnSHKQd7bqz7k9203YQTJMQcWlE-hQEnGXY_C-GzV3-1_bIELwIEwnLweC0IItas-c8YiCYSIu40bKCGvYC5qHFZgN2lKl4Jk1EcFh3T60rBn4WX9uV-CrCTZSDkOdvXx4030al_HWM_DDxHcBURb5ofURvMb3K5V4zSn3h-zfS4P6yppISAiT8uoBy1wbtFeVBdzJ9kOvEn22tNN48SG2sfq9Hu4tECOvET1qlBjidsMLiOupeMjhTwRsRst3-sH7N1QGrzHnG_e_xKz3ppcksiM7YdHsWhuQ5b2k6zEIc7FtZmiDvY9hZpVyu7pSTojr4hNYeFtS5Qhy0OFir0EHVkcuu1UfehVgIJlqxUn5LtsYzjP8WNEAK7KNVSDgq8rU3-wm9kI_Ddd_nMGY04GljfF4HE7X37MQg-I8Occhov0EVeVu7FJEoJlS5leX0hR-ccvpm848DIUVRpo8t1Ohk5S8O7oSdfR5v1HNn6dewXaKlFj8dZ_2MEeDFGsufCu5ZTdULU59zdEJiHBRqcyB1RFEoXbj0u1ex_TVtkKVdSQbemZ-up17VOEM4-Xz4TGgcVw_PI7miYtMsyxReqUvSdYaiQ32h3hfg

export AWS_DEFAULT_REGION=us-east-1
export AWS_ACCESS_KEY_ID=ASIAUQ2ZZ55RW67WUW7M
export AWS_SECRET_ACCESS_KEY=Doco7ubatcFbeftzTCEknSM8+XlCsvAcOfklHe2n
export AWS_SESSION_TOKEN=IQoJb3JpZ2luX2VjECYaCXVzLWVhc3QtMSJHMEUCID743pVInfVdufoZE0iKKbHR6WxoAisolWt0MHGu3jVUAiEAm6jOVXPzcRUQbMMhgR4RGJVqI45Vmmv22bm7DgAgbLgqlwIILxAAGgwzMTEwMzY2MDQyNTkiDCS0zqLOuLlMwoCiByr0AUO4MRcVVeW0od2M+8A7zv1Ux8Fhco6yI6g9CbzZgbqoP9wdCHJmrnbBk0C831hUbOZVs/2Auo8wKytT4VHJCJ4/du9vDkXKWfezOK3dTz13UlzEuI3QX6QszxhS8BjADdi0WVygn0zdTZqPfgH/G4dHRHR8XwCGkW513XW1rkP6cDP/GvRf//l3VvvxsSZtr457mqE78S9aaBX3vSkiMv3ZiahCCO7klmhzBdGcc5J70ztjSXZnfgwcHrgIa14SvmgvxHD5gZoncB45hISRCmkBPBOFfdvmxHz0XLKJ1yfS2Peo4oszL7vFULoCqps5VHYgVp4wsM6kogY6nQH5MzJtm98wk6b2aaJ2AGpMVBzVksJLl5fN35731fbRoJ4HlbHxqSvBl9c1OfiTjSqsIP4I1INKKmq/bBrElEBBPZDYI5cn7WPJkPjug7Fs08/fsb56Bc4pxJIkVH3KuqX7MiTDLu3O2b9/57QYk/hBoHzJFFEWNIWKn1NU4iWbMT0ENv6Cutr3u2yPaP3nGjqu1ItNcP8H/6dJz9zg

Action couples visuals
Filter filters one/many/all, can cascade

## networking
[Connecting to a VPC with Amazon QuickSight - Amazon QuickSight](https://docs.aws.amazon.com/quicksight/latest/user/working-with-aws-vpc.html)
By creating a VPC connection in QuickSight, you're adding an elastic network interface in your VPC. This network interface allows QuickSight to exchange network traffic with a network instance within your VPC
The QuickSight network interface alone doesn't give QuickSight direct access to your databases. The VPC connection works only for the QuickSight data sources that are configured to use it.
There is only one QuickSight network interface for each VPC. It is created on the subnet that you set up in the QuickSight VPC connection.
