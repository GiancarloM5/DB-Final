# DB-Final
Java 228 final
//Giancarlo Massoni
//5/18/20
//CreateBookStoreDB
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class CreateBookStoreDB {

	public static void main(String[] args) {
		
		final String DB_URL ="jdbc:derby:Personnel;create=true";
		
		try
	      {
		         
		         Connection conn = DriverManager.getConnection(DB_URL);
		         
		         dropTables(conn);
		         
		         buildBooksTable(conn);
		         
		         Statement stmt = conn.createStatement();
		         
		         
		         String sqlStatement = "SELECT * FROM Books ORDER BY Title";
		         
	
		         ResultSet result = stmt.executeQuery(sqlStatement);
		         
		         System.out.println("\tCurrent Inventory");
		         System.out.println("Title\t\t\tQuantity\t");
		         System.out.println("------------------------------------");
		       
		         while (result.next())
		         {
		            System.out.println(result.getString("Title") + " " + result.getString("Quantity"));
		                              
		         }
		         
		         UpdateInventory(conn);
		         
		         System.out.println("\n\tUpdated Inventory");
		         System.out.println("Title\t\t\tQuantity\t");
		         System.out.println("------------------------------------");
		       
		         ResultSet result1 = stmt.executeQuery("SELECT * FROM Books ORDER BY Quantity");
		         
		         while (result1.next())
		         {
		            System.out.println(result1.getString("Title") + " " + result1.getString("Quantity"));
		                              
		         }
		         
		         
		         
		         
		         conn.close();
		      }
		      catch(Exception ex)
		      {
		         System.out.println("ERROR: " + ex.getMessage());
		      }
		
	}
	
	
	public static void buildBooksTable(Connection conn)
	{
		try
		{
     
         Statement stmt = conn.createStatement();
         
			
         stmt.execute("CREATE TABLE Books" + "( Title CHAR(30), " + " ISBN CHAR(15) NOT NULL PRIMARY KEY, " + " Quantity DOUBLE, " + "Price DOUBLE )");
							 
			
			stmt.execute("INSERT INTO Books VALUES (" + "'The Cat in the Hat', " + "'0-394-80001-X'," + "3, " + "7.99 )");
			
			stmt.execute("INSERT INTO Books VALUES (" + "'How the Grinch Stole Christmas', " + "'0-394-80079-6'," + "22, " + "13.59 )");
			
			stmt.execute("INSERT INTO Books VALUES (" + "'The Lorax', " + "'0-394-82337-D'," + "1, " + "10.99 )");		
			
		}
		catch (SQLException ex)
      {
         System.out.println("ERROR: " + ex.getMessage());
      }
		
		}
	
	
	public static void dropTables(Connection conn)
	{
	
		try
		{
			
			Statement stmt  = conn.createStatement();;
		
			
			try
			{
	         
	         stmt.execute("DROP TABLE Books");
								
			}
		
			catch(SQLException ex)
			{
	   
			}
			
	      	
		}catch(SQLException ex)
		
		{
			      System.out.println("ERROR: " + ex.getMessage());
					ex.printStackTrace();}
				}


	public static void UpdateInventory(Connection conn) throws SQLException
	{
		try
		{
			Statement stmt = conn.createStatement();
			stmt.executeUpdate("UPDATE Books SET Quantity = Quantity + 3");
			conn.commit();
		}
		catch(SQLException e)
		{
			conn.rollback();
		}	
	}
	







}
