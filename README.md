# CreditsAndGrants

## Generate Database

1. Unzip the file `database/data/IDA_Credits_and_grants.zip`
2. Run Postgres in a container:
   ```
   docker container run --rm -d \
     --name postgres \
     -v ${PWD}/data:/data \
     -v pgdata:/var/lib/postgresql/data \
     -p 5432:5432 \
     postgres:11.2-alpine
   ```
3. Exec into the running Postgres container:

   ```
   docker container exec -it postgres \
     psql -U postgres
   ```

4. In the "psql" editor create a table "credits_and_grants" as follows:

   ```
   CREATE TABLE credits_and_grants(
       End_of_Period timestamp,
       Credit_Number varchar(10),
       Region varchar(50),
       Country_Code varchar(2),
       Country varchar(50),
       Borrower varchar(50),
       Credit_Status varchar(20),
       Service_Charge_Rate numeric(15,2),
       Currency_of_Commitment  varchar(3),
       Project_ID  varchar(10),
       Project_Name  varchar(50),
       Original_Principal_Amount numeric(15,2),
       Cancelled_Amount numeric(15,2),
       Undisbursed_Amount numeric(15,2),
       Disbursed_Amount numeric(15,2),
       Repaid_to_IDA numeric(15,2),
       Due_to_IDA numeric(15,2),
       Exchange_Adjustment numeric(15,2),
       Borrowers_Obligation numeric(15,2),
       Sold_3rd_Party numeric(15,2),
       Repaid_3rd_Party numeric(15,2),
       Due_3rd_Party numeric(15,2),
       Credits_Held numeric(15,2),
       First_Repayment_Date timestamp,
       Last_Repayment_Date timestamp,
       Agreement_Signing_Date timestamp,
       Board_Approval_Date timestamp,
       Effective_Date timestamp,
       Closed_Date timestamp,
       Last_Disbursement_Date timestamp
   );
   ```

5. Import the data from the file into Postgres:

   ```
   COPY Credits_and_Grants FROM '/data/    IDA_Credits_and_Grants.csv' DELIMITER ',' CSV     HEADER;
   ```

6. Add a primary key to the table:

   ```
   ALTER TABLE credits_and_grants ADD COLUMN id    SERIAL PRIMARY KEY;
   ```

7. You can optionally use "pgadmin4" to view and edit the database and the data in it. Run the following container:

   ```
   docker container run --rm -d \
       -p 8080:80 \
       --net tu-connect_confluent \
       -e PGADMIN_DEFAULT_EMAIL=student@confluent.io \
       -e PGADMIN_DEFAULT_PASSWORD=TopSecret \
       dpage/pgadmin4
   ```

   and then in a browser navigate to "192.168.99.100:8080"

8. Login with `student@confluent.io` and password `TopSecret`.
