### 15-digit-to-18-digit-IDs-Salesforce
Remedy to getting 18 digit IDs from Salesforce - Required for data export out of SF to any other system.

1.  For each object you need to export, you need to create a formula field that uses the SAFE Id.
2.  Create a custom field:
  - Formula field
  - Result: Text
  - Formula = CASESAFEID (Id)


You can also enter the following into a Formula Field:

Id & MID("ABCDEFGHIJKLMNOPQRSTUVWXYZ012345",(     IF(FIND(MID(Id,1,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,1,0)     +IF(FIND(MID(Id,2,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,2,0)     +IF(FIND(MID(Id,3,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,4,0)     +IF(FIND(MID(Id,4,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,8,0)     +IF(FIND(MID(Id,5,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,16,0)     )+1,1) & MID("ABCDEFGHIJKLMNOPQRSTUVWXYZ012345",(     IF(FIND(MID(Id,6,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,1,0)     +IF(FIND(MID(Id,7,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,2,0)     +IF(FIND(MID(Id,8,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,4,0)     +IF(FIND(MID(Id,9,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,8,0)     +IF(FIND(MID(Id,10,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,16,0)     )+1,1) & MID("ABCDEFGHIJKLMNOPQRSTUVWXYZ012345",(     IF(FIND(MID(Id,11,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,1,0)     +IF(FIND(MID(Id,12,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,2,0)     +IF(FIND(MID(Id,13,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,4,0)     +IF(FIND(MID(Id,14,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,8,0)     +IF(FIND(MID(Id,15,1), "ABCDEFGHIJKLMNOPQRSTUVWXYZ")>0,16,0)     )+1,1)

###How to calculate the 18 Digit Id from 15 Digit Id programatically:

//Our 15 Digit Id
String id = '00570000001ZwTi' ;

string suffix = '';
integer flags;

for (integer i = 0; i < 3; i++) {
          flags = 0;
          for (integer j = 0; j < 5; j++) {
               string c = id.substring(i * 5 + j,i * 5 + j + 1);
               //Only add to flags if c is an uppercase letter:
               if (c.toUpperCase().equals(c) && c >= 'A' && c <= 'Z') {
                    flags = flags + (1 << j);
               }
          }
          if (flags <= 25) {
               suffix = suffix + 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.substring(flags,flags+1);
          }else{
              suffix += '012345'.substring(flags - 26, flags-25);
          }
     }

//18 Digit Id with checksum
System.debug(' ::::::: ' + id + suffix) ;

https://srinusfdc.blogspot.com/2012/11/difference-between-15-digit-id-and-18.html
