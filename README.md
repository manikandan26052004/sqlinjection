# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/d4caf2d4-3c25-448b-8550-c28cfe4bb257)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser. 

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/4d7ca9aa-6e18-4e89-8fe5-25cc88301f19)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/105b318d-2d1d-44af-b816-e9c550282ec0)

Click on the menu Login/Register and register for an account 
![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/76d03808-85fb-401e-8ba3-746754b1424c)

Click on “Create Account” to display the following page: The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/1ee8717e-915c-41e9-8e1f-fdae8e55e886)

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/e625f999-74cb-43bc-81c0-8849151664b4)

Click “Login”. The logged in page will show as below:
![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/472468d9-4705-478a-a462-365571c0ed89)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/6708ec2f-509a-4781-bc8b-e170950f8772)

Click the login button and you will see it enter into the administrator page.
![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/f2ff04c4-7bd6-45eb-901d-57fe23cb908d)

Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/c629f439-b627-4f5f-bf33-001af9e387bd)
![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/b3928d99-883e-44ca-92ef-42fb725b025f)
![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/e7cc84ae-baf4-4f30-94f4-884b809866c5)
![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/cebe3aec-f0ff-475e-976a-93acc1fbb7d6)
![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/0bd5fda7-2d34-471e-adaa-52668dc5b7eb)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/52cba5f2-f1cd-4702-b599-37a1e9699ae9)
Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/04b072ef-b821-4247-9e73-010f86615d88)
After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/fc391093-6ad3-42f7-bc70-bec89d46d1e3)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/2c6e8b31-b0dd-4cd8-b6e7-1f5481801866)
As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/828b695f-edcc-4a73-ad74-64139c433a05)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/fc02c9d5-bc94-4fc6-b828-8cad8920850a)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/f9426566-a8fb-449b-b531-7f83c39270e4)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/6861a3df-5dbb-4c2c-afd7-13deb54bdb23)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/0f46bcd8-1e6a-4288-8bf6-a93486d9f7f1)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/d7874025-ad3f-441b-a453-e0ef41793aba)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/manikandan26052004/sqlinjection/assets/121999845/f3896557-4046-4503-8ee8-270cf85fcecb)

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
