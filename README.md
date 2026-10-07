# Oracle PDB Assignment II

Student: Jeannette
Student ID: 27745

Overview

This assignment demonstrates practical understanding of Oracle Multitenant
Architecture — creating, opening, and deleting Pluggable Databases (PDBs),
creating a user inside a PDB, and using Oracle Enterprise Manager (OEM).

 Oracle Environment Used

- Oracle Database 21c — Multitenant (CDB name: ORCL)
- Oracle SQL Developer 23.x
- Oracle Enterprise Manager Express (https://localhost:5500/em)

Task 1 — Create a PDB and a User

PDB Name: je_pdb_27745
User inside PDB: jeannette_plsqlaua_27745

 Commands used

    CREATE PLUGGABLE DATABASE je_pdb_27745
      ADMIN USER je_admin IDENTIFIED BY Oracle123
      ROLES = (DBA)
      FILE_NAME_CONVERT = (
        'C:\DBHOME\ORADATA\ORCL\PDBSEED\',
        'C:\DBHOME\ORADATA\ORCL\je_pdb_27745\'
      );

    ALTER PLUGGABLE DATABASE je_pdb_27745 OPEN;
    ALTER PLUGGABLE DATABASE je_pdb_27745 SAVE STATE;

    ALTER SESSION SET CONTAINER = je_pdb_27745;

    CREATE USER jeannette_plsqlaua_27745
      IDENTIFIED BY Oracle123
      QUOTA UNLIMITED ON USERS;

    GRANT CONNECT, RESOURCE, DBA TO jeannette_plsqlaua_27745;

Task 2 — Create and Delete a Temporary PDB

Temp PDB Name: je_to_delete_pdb_27745

Commands used

    CREATE PLUGGABLE DATABASE je_to_delete_pdb_27745
      ADMIN USER je_tmp_admin IDENTIFIED BY Oracle123
      FILE_NAME_CONVERT = (
        'C:\DBHOME\ORADATA\ORCL\PDBSEED\',
        'C:\DBHOME\ORADATA\ORCL\je_to_delete_pdb_27745\'
      );

    ALTER PLUGGABLE DATABASE je_to_delete_pdb_27745 OPEN;
    SHOW PDBS;

    ALTER PLUGGABLE DATABASE je_to_delete_pdb_27745 CLOSE IMMEDIATE;
    DROP PLUGGABLE DATABASE je_to_delete_pdb_27745 INCLUDING DATAFILES;
    SHOW PDBS;

 Task 3 — Oracle Enterprise Manager (OEM)

- OEM URL: https://localhost:5500/em
- Logged in as sys (SYSDBA)
- Dashboard shows CDB ORCL and both PDBs (ORCLPDB, JE_PDB_27745)

 Challenges Faced and Solutions

1. ORA-65005 on first CREATE PLUGGABLE DATABASE attempt
   The FILE_NAME_CONVERT used the ORCLPDB folder, but Oracle templates 
   new PDBs from PDB$SEED. Fixed by changing the source folder to 
   `C:\DBHOME\ORADATA\ORCL\PDBSEED\`.

2. ORA-01031: insufficient privileges
   The `sys_root` connection was actually authenticated as SYSTEM, not 
   SYS AS SYSDBA. Fixed by editing the connection properties and setting 
   the Role dropdown to SYSDBA.


 Submission Details

- Repository Link: https://github.com/jeannette-12/oracle_pdb_ass_II_27745_jeannette
- PDB Name Created: je_pdb_27745
