```
  <connectionStrings>
    <add name="SettingDB" connectionString="server=redninjausersetting.mysql.database.azure.com;User ID=redninja@redninjausersetting;Password=Sql123123;database=red_ninja_user;"
         providerName="System.Data.SqlClient" />
  </connectionStrings>
```



    namespace EntityFilter.Controllers
    
    {
    
        using System;
    
        using System.IO;
    
        using System.Net;
    
        using System.Net.Http;
    
        using System.Threading.Tasks;
    
        using System.Web;
    
        using System.Web.Http;
    
        using System.Web.Http.Cors;
    
        using MySql.Data.MySqlClient;
    
        using System.Web.Configuration;
    
    
    [EnableCors(origins: "*", headers: "*", methods: "*")]
    public class MySqlController : ApiController
    {
    
        [HttpPost]
        [Route("setting/getvalue")]
        public HttpResponseMessage GetUserSetting([FromBody] EmailInfo info)
        {
            try
            {
                string emailAddress = info.email_address;
                string sqlQuery = string.Format("SELECT * FROM red_ninja_user where email_address = '{0}'", emailAddress);
                string resultContent = "MainPage";
    
                MySqlConnection mySqlConn = new MySqlConnection(WebConfigurationManager.ConnectionStrings["SettingDB"].ConnectionString);
                mySqlConn.Open();
                MySqlCommand cmd = new MySqlCommand(sqlQuery, mySqlConn);
    
                MySqlDataReader rdr = cmd.ExecuteReader();
                if (rdr.HasRows)
                {
                    while (rdr.Read())
                    {
                        resultContent = (string)rdr[1];
                    }
                    rdr.Close();
                }
                else
                {
                    rdr.Close();
                    string sqlInsert = string.Format("INSERT INTO red_ninja_user (email_address, setting) value('{0}', 'MainPage')", emailAddress);
                    MySqlCommand cmdInsert = new MySqlCommand(sqlInsert, mySqlConn);
                    cmdInsert.ExecuteNonQuery();
                }
                mySqlConn.Close();
    
                return Request.CreateResponse(HttpStatusCode.OK, resultContent);
            }
            catch (Exception ex)
            {
                return Request.CreateResponse(HttpStatusCode.InternalServerError, ex.Message);
            }
        }
    
        [HttpPost]
        [Route("setting/setup")]
        public HttpResponseMessage UpdateUserSetting([FromBody] Info info)
        {
            string emailAddress = info.email_address;
            string setting = info.setting;
    
            try
            {
                string sqlUpdate = string.Format("INSERT INTO red_ninja_user (email_address, setting) value('{0}', '{1}') ON DUPLICATE KEY UPDATE setting = '{1}'", emailAddress, setting);
                MySqlConnection mySqlConn = new MySqlConnection(WebConfigurationManager.ConnectionStrings["SettingDB"].ConnectionString);
                mySqlConn.Open();
                MySqlCommand cmd = new MySqlCommand(sqlUpdate, mySqlConn);
                MySqlDataReader rdr = cmd.ExecuteReader();
                while (rdr.Read())
                {
                }
                rdr.Close();
                mySqlConn.Close();
                return Request.CreateResponse(HttpStatusCode.OK, setting);
            }
            catch (Exception ex)
            {
                return Request.CreateResponse(HttpStatusCode.InternalServerError, ex.Message);
            }
        }
    
        public class Info
        {
            public string email_address { get; set; }
            public string setting { get; set; }
        }
    
        public class EmailInfo
        {
            public string email_address { get; set; }
        }
    }
    }
