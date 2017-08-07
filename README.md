# javawebdemo
学习demo
--JDBC登录验证
--登录页面
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<html>
  
  <body>
  <form action="servlet/LoginServlet" method="post">
   	用户名:<input type="text" name="uname"/><br>
   	密码:<input type="password" name="pass"/><br>
   	<input type="submit" value="提交">
   	</form>
  </body>
</html>
--servlet处理
package jdbc.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class LoginServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		request.setCharacterEncoding("UTF-8");
		response.setCharacterEncoding("UTF-8");
		
		String name=request.getParameter("uname");
		String pass=request.getParameter("pass");
		
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection conn=DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:orcl","name","password");
			Statement st=conn.createStatement();
			String sql = "select * from login where name='"+name+"' and pass='"+pass+"'";
			System.out.println(sql);
			ResultSet rs=st.executeQuery(sql);
			if(rs.next()){
				System.out.println("a");
				response.sendRedirect("../success.jsp");
			}else{
				System.out.println("b");
				request.getRequestDispatcher("../error.jsp").forward(request, response);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}		
	}
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

	this.doGet(request, response);
	}

}
--成功页面
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
 
  
  <body>
   	<h3 ><font color='green'>登录成功</font></h3>
  </body>
</html>
--失败页面
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
  
  <body>
    <h3><font color='red'>用户名或密码错误</font></h3>
  </body>
</html>
