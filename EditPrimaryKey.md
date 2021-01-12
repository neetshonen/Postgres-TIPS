# EditPrimaryKey
>Update our DB and change the Primary key , and delete a column....
>there are data with this primary key that without it would be duplicates and will violate the new Primary key
>So we have to BACKUP and COPY the stdin ON CONFLICT DO NOTHING

### Step 1  
  Dump table to PLAIN and UTF8 .
### Step 2  
  Open the file we dumpped and follow the same structure of it 
  
  At the bottom there is the primary key ,delete the one we dont want and delete the one with the column we want to get rid of (if its in primary key)
  Edit the top CREATE TABLE function so that we dont have the column we want to delete
  Go to the DB lines and replace the values of the column we want to delete with nothing (you can also use other methods for this) to delete all the useless values
  Edit the COPY function to copy to a temp table with the same structure as the current one
  ```
  create temporary table __copy as (select * from public.table_name limit 0);
  COPY __copy ....
  ```
  At the end of the file after the initialization of the primary key paste the line (obviously you can change the ON CONFLICT doings)
  ```
  INSERT INTO public.table_name SELECT * FROM __copy on conflict do nothing;
  ```
### Step 3  
  go to CMD inside the postgres bin
  ```
  $ psql -U db_user db_name < dump_name.sql
  ```
You are done!
