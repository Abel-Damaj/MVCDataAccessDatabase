# MVCDataAccessDatabase
Sample of Data Access ASP.net 

Asp.net /https://www.youtube.com/watch?v=bIiEv__QNxw/

General Rules to follow when Data Access in C#:

1. First, Creating ASP.net MVC Web Application.
2. Make a new project and create Class Library .net framework
3. Add one more project which is a sql database. 
4. Start with Models. by adding a class . <<For UI>>
5. After, modifiying the Controller. 
6. Add, a view for what you did in the 5
7. Securitize the Model of UI
8. We modify the controller once again to captcher the data entered by user.
9. [HttpPost] is to post data. otherwise, its gonna take the first Sign up to get data back.
10.[ValidateAntiForgeryToken] One Half in the controller, and the other in the view @Html.AntiForgeryToken()
11. if (ModelState.IsValid) meaning is the model variable is in the exact form as the Model? // Double checking to avoid user from turning JavaScript off in his browser.
12.Now, its posting data to our MVC Controller, the next step is to save data into a database.
13.The next step is the BusinessLogic -> starting from DataLibrary..
14.Before 13, start with the SQL Database created in the 3. start by adding the following Folders: dbo -> Tables, then Add a Table. 
15.Then Publish the database, you created in the 14.
16.Add  <connectionStrings><add name="MVCDemoDB" connectionString="..."></add></connectionStrings> To web.config under the <configuration>
17.Inside DataLibrary, we make three folders. (1) Models "For Our Data Back End, Name it as the UI Model"-(2)-DataAccess "Where to Make Access to the data of SQLDatabase, you add more Dapper from NUGET, "-(3)BusinessLogic ""
18.Add to SqlDataAccess.cs the followings: using Dapper; using System.Configuration; using System.Data; using System.Data.SqlClient;
19.we add at least the following methods as the following:
        (1) To make the connection with the database: 
            public static string GetConnectionString(string connectionName = "MVCDemoDB")
        {
            return ConfigurationManager.ConnectionStrings[connectionName].ConnectionString;
        }

        (2) To Load Data from the database: 
        public static List<T> LoadData<T>(string sql)
        {
            using (IDbConnection cnn = new SqlConnection(GetConnectionString()))
            {
                return cnn.Query<T>(sql).ToList();
            }
        }
        
        (3) To Save Data into a Database: 
        public static int SaveData<T>(string sql, T data)
        {

            using (IDbConnection cnn = new SqlConnection(GetConnectionString()))
            {
                return cnn.Execute(sql, data);
            }

        }
20.Add a new class to our BusinessLogic Folder. Call it "xxxProcessor"
21.Inside the class made in 20. make the class static. and add a new method pass all the properties of the model.
22.
        public static int CreateEmployee(int employeeId, string firstName, string lastName, string emailAddress)
        {
            EmployeeModel data = new EmployeeModel
            {
                EmployeeId = employeeId,
                FirstName = firstName,
                LastName = lastName,
                EmailAddress = emailAddress
            };

            string sql = @"INSERT INTO dbo.Employee(EmployeeId, FirstName, LastName, EmailAddress) 
                           VALUES (@EmployeeId, @FirstName, @LastName, @EmailAddress)";

            return SqlDataAccess.SaveData(sql, data);
        }

        public static List<EmployeeModel> LoadEmployee()
        {
            string sql = @"SELECT Id, EmployeeId, FirstName, LastName, EmailAddress FROM dbo.Employee;";

            return SqlDataAccess.LoadData<EmployeeModel>(sql);
        }
    }
23.We back to our MVC and we add a reference to the DataLibrary. 
24.We go to our Home Controller and we add -> using DataLibrary;
25.Inside the validation of 11. we add our BusinessLogic xxxProcessor


