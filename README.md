# Digital-Forensincs

/***********************************************************************
* Program:
*    Assignment 02, Digital Forensincs   
*    Brother Dudley, CS165
* Author:
*    Alberto Contreras
* Summary: 
*     A program that helps determine what additional 
*     data might have been involved in the breach. 
*     scan through that log to identify users who accessed 
*     files in a particular window of time.
*
*    Estimated:  6.0 hrs   
*    Actual:     3.0 hrs
*      Being able to read the file when I saved it as a string.
************************************************************************/
#include <iostream>
#include <iomanip>
#include <string>
#include <fstream>
#include  <cstring>
using namespace std;

//This struct will help me read the file I will do the investigation on.
struct AccessRecord
{
   string userName;
   string fileName;
   long timeStamp;
};

/******************************************************************
* We ask the user for the name of the file, of the log records.
******************************************************************/
void getFileName(string & fileName)
{
   cout << "Enter the access record file: ";
   getline(cin,fileName);
   return;
}

/******************************************************************
*   We read the file and store it in an array struct variable.
******************************************************************/
int readFile(string fileName, AccessRecord records[])
{
    int count = 0; 
    
    ifstream fin((fileName.c_str())); //I had to cast the string
    if (fin.fail()) // always check to see if the file correctly opened
       return 0;    // if we failed, do not continue on
       
    //Here I read and save the information inside the file.
    while (fin >> records[count].fileName >> records[count].userName 
    >> records[count].timeStamp )
    {
       count++;
    }
       
    // close the file
    fin.close(); // do not forget to close the file when finished
    
    return count + 1; // I added one so it can represent the number of actual records

}

/******************************************************************
* It asks the log on times constraints.
******************************************************************/
void promptTime(long & startTime, long & endTime)
{
   cout << "\nEnter the start time: ";
   cin  >> startTime;
   
   cout << "Enter the end time: ";
   cin  >> endTime;
   
}

/******************************************************************
* Displays the records of our criteria.
******************************************************************/
void displayRecord(long startTime, long endTime,  AccessRecord records[], int count)
{
   cout << "\nThe following records match your criteria:\n";
   cout << endl;
   cout << setw(15) << "Timestamp" << setw(20) << " File" << setw(20) 
   << " User" << endl;
   cout << "--------------- ------------------- -------------------\n";
   
   
   for(int i = 0; i < count; i++)
   {  
      if ( startTime <= records[i].timeStamp && records[i].timeStamp <= endTime)
      {
         //display 
         cout << setw(15) << records[i].timeStamp << setw(20) 
         << records[i].fileName
         << setw(20) << records[i].userName << endl;           
      }      
    }
    
    cout << "End of records\n";
}

/**********************************************************************
 * Add text here to describe what the function "main" does. Also don't forget
 * to fill this out with meaningful text or YOU WILL LOSE POINTS.
 ***********************************************************************/
int main()
{
   AccessRecord records[500];
   long startTime;
   long endTime;
   
   string fileName;
   getFileName(fileName);
   int count = readFile(fileName, records);
   promptTime(startTime, endTime);
   displayRecord(startTime, endTime, records, count);

   
   return 0;
}
