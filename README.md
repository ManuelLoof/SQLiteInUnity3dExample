# SQLiteInUnity3dExample
## Simple example implemention of SQLite in Untiy3d.

This is a simple step by step example how to use SQLite in Unity3d. The complete implementation you will find here in the repository.

SQLLite is a lightweight, file-based database engine under public domain license. More information you find here https://www.sqlite.org/.

It is a convenient way of implementing a simple database in Unity.

This is a very simple step example how to implement SQLite in Unity3d.

## Implementation

### **Step 1:** Implemented the SQLite library

- Download the SQLLite precompiled binaries from https://www.sqlite.org/download.html. In my case, I used the precompiled binaries for Windows (sqlite-dll-win64-x64-3180000.zip).
- Create a plugins folder in your Unity3d project.
- Copy the files "sqlite3.def", "sqlite3.dll" from the zip archive into the "plugins" folder.
- Copy from your unity program folder "C:\Program Files\Unity 2017.1.0b2\Editor\Data\Mono\lib\mono\2.0" the files "Mono.Data.SqliteClient.dll" and "System.Data.dll" into the plugins folder.
- Your "plugins" must look like this:
    - plugins
        - Mono.Data.SqliteClient.dll
        - sqlite3.def
        - sqlite3.dll
        - System.Data.dll



### **Step 2:** Scripting in Unity

Here is a simple example C# script that creates a new table, adds data and read the datasets.

    /// <summary>
    /// Example script thats creates a table, add datasets and read them.
    /// </summary>
    public void SimpleExampleScript()
    {

        // The name of the db.
        string dbName = "URI=file:Example.db";

        // Open the db connection.
        using (var connection = new SqliteConnection(dbName))
        {
            connection.Open();

            // Create an sql command that creates a new table.
            using (var command = connection.CreateCommand())
            {
                // Drop the table if we call this mathode earlier.
                command.CommandText = "DROP TABLE IF EXISTS highscore;";
                command.ExecuteNonQuery();

                command.CommandText = "CREATE TABLE highscore (name VARCHAR(20), score INT);";
                command.ExecuteNonQuery();


                // Create test datasets
                command.CommandText = "INSERT INTO highscore (name, score) VALUES ('Ash', 9000);";
                command.ExecuteNonQuery();

                command.CommandText = "insert into highscore (name, score) values ('Evil Dead', 12064);";
                command.ExecuteNonQuery();

                command.CommandText = "insert into highscore (name, score) values ('chainsaw', 15000);";
                command.ExecuteNonQuery();

                // Read the datasets
                command.CommandText = "select * from highscore order by score desc;";
                using (IDataReader reader = command.ExecuteReader())
                {
                    while (reader.Read())
                        Debug.Log("Name: " + reader["name"] + "\tScore: " + reader["score"]);

                    reader.Close();
                }
            }
            connection.Close();
        }

    }
