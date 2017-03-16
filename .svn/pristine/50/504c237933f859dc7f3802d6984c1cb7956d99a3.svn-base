using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Configuration;
using System.Data;
using System.Data.SqlClient;

namespace PaperEvaluationSqlHelper
{
   public class SQLHelper
    {
        //private static SqlConnection Conn;
        /// <summary>
        /// 创建连接字符串并打开连接
        /// </summary>
        /// <returns></returns>
       public static SqlConnection CreateConnection()
        {
            string connstr = ConfigurationManager.ConnectionStrings["ConStr"].ConnectionString;
            SqlConnection Conn = new SqlConnection(connstr);
            if (Conn.State == ConnectionState.Closed) { Conn.Open(); }
            return Conn;
        }
        /// <summary>
        /// 创建一个新的连接字符串并打开连接
        /// </summary>
        /// <returns></returns>
       public SqlConnection CreateNewConnection()
        {
            string connstr = ConfigurationManager.ConnectionStrings["ConStr"].ConnectionString;
            SqlConnection conn = new SqlConnection(connstr);
            if (conn.State == ConnectionState.Closed)
            {
                conn.Open();
            }
            return conn;
        }
        /// <summary>
        /// 创建执行Sql语句的对象
        /// </summary>
        /// <param name="sql"></param>
        /// <returns></returns>
       private static SqlCommand CreateCommond(string sql)
        {
            CreateConnection();
            return new SqlCommand(sql, CreateConnection());
        }
        /// <summary>
        /// 执行SQL语句并返回结果集
        /// </summary>
        /// <param name="sql"></param>
        /// <returns></returns>
       public static IDataReader ExecDataReader(string sql)
       {
           using (SqlCommand com = CreateCommond(sql))
           {
               SqlDataReader dr = com.ExecuteReader(CommandBehavior.CloseConnection);
               return dr;
           }

       }
        public IDataReader ExecDataReader(string sql, SqlConnection con)
        {
            using (SqlCommand com = new SqlCommand(sql, con))
            {
                SqlDataReader dr = com.ExecuteReader(CommandBehavior.CloseConnection);
                return dr;
            }
        }
        public int ExecNonScale(string sql)
        {
            using (SqlConnection conn = CreateConnection())
            {
                using (SqlCommand com = new SqlCommand(sql, conn))
                {
                    int result = com.ExecuteNonQuery();
                    conn.Close();
                    return result;
                }
            }
        }
        public string ExecScale(string sql)
        {
            try
            {
                using (SqlConnection conn = CreateConnection())
                {
                    using (SqlCommand com = new SqlCommand(sql, conn))
                    {
                        string result = com.ExecuteScalar().ToString();
                        conn.Close();
                        return result;
                    }
                }
            }
            catch (Exception e)
            {
                return null;
            }
            
        }
        public DataTable GetDataTable(string sql)
        {
            using (SqlConnection conn = CreateConnection())
            {
                using (SqlCommand com = new SqlCommand(sql, conn))
                {
                    SqlDataAdapter da = new SqlDataAdapter(com);
                    DataSet ds = new DataSet();
                    da.Fill(ds, "table1");
                    return ds.Tables[0];
                }
            }

        }
        public DataSet GetDataSet(List<string> sqlList)
        {
            using (SqlConnection conn = CreateConnection())
            {

                DataSet ds = new DataSet();
                foreach (string sql in sqlList)
                {
                    SqlCommand com = new SqlCommand(sql, conn);

                    SqlDataAdapter da = new SqlDataAdapter(com);

                    da.Fill(ds, "table" + sqlList.IndexOf(sql) + 1);
                    com.Dispose();
                }
                return ds;
            }
        }
    }
}
