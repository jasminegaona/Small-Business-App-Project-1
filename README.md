/*# Small-Business-App-Project-1
Program Name: Coffee &amp; Tea POS System Description: This program is designed by J-Code for the Point of  Sales (POS) system for cashiers working at Coffee &amp; Tea. The program  will calculate the price for the total item(s) chosen and ultimately display the customer's receipt, as well as on the Reciept.txt file.
*/
/***************************************
Final CIS17A Project 
Course: 20288
Coffee Shop Program
Jasmine Gaona & Julissa Mota
Team name: J-Code
February 13, 2020

Program Name: Coffee & Tea POS System
Description: This program is designed by J-Code for the Point of 
Sales (POS) system for cashiers working at Coffee & Tea. The program 
will calculate the price for the total item(s) chosen and ultimately
display the customer's receipt, as well as on the Reciept.txt file.
****************************************/

//Processors
#include <iostream>     //for input and output
#include <string>       //for strings
#include <iomanip>      //for setprecision
#include <vector>       //for vectors
#include <fstream>      //to write out to file
#include <climits>      //for INT_MAX
#include <cstdlib>      //for exit 

using namespace std;

void menuOption();                          //function menuOption Prototype
void desiredDrinkSize(vector<string>);      //function desiredDrink Prototype
void totalCost(vector<double>);             //function total Prototype
void list(vector<string>);                  //function itemsList Prototype  
void CustomerReciept();                     //function CustomerReciept to output data to file  
void clear ();

const int totalItems=10;
string items[totalItems] = {"Latte" ,       //Array : items 
          "Cappuccino" , 
          "Frappuccino" , 
          "Americano" , 
          "Iced Coffee" , 
          "Green Tea" , 
          "Passion Fruit Tea", 
          "Peach Tea" , 
          "Black Tea" , 
          "quitOption" };

const int totalItemPrices = 3;           
double price[totalItemPrices] = {3.75,      //Array : price
                                 4.50,
                                 5.25 };   

char drinkSize;                             //Global variable declarations 
double totalPerDrink;

char again;

string customerName;

int option;
int object;

const double taxRate = 0.0725;
    double subTotal;        
    double total;
    double pay;
    double change;
    double tax;

vector<double>reciept;                    //Defining global vectors
vector<string>itemsList;
vector<string>drinks;

/////////////////////////////////////////////////////////////////

int main ()                                //Function header for main
{
  
  do
  {


    //Introduction to Coffee & Tea POS System and the menu
    cout << "\n\n\t\t\t\tCoffee & Tea POS System" <<endl;
    cout << "\t\t\t*************************************" << endl;
    cout << "\n\n\nEnter customer's name: ";
    cin>> customerName;
    cout << "\n\nBegin order for: " << customerName << "\n\n\n";

     cout << "Choose item/s number that customer is ordering: \n";
      cout << "------------------------------------------------------" << endl;
      cout << "1.Latte" << endl;
      cout << "2.Cappuccino" << endl;
      cout << "3.Frappuccino" << endl;
      cout << "4.Americano" << endl;
      cout << "5.Iced Coffee" << endl;
      cout << "6.Green Tea" << endl;
      cout << "7.Passion-fruit Tea" << endl;
      cout << "8.Peach Tea" << endl;
      cout << "9.BlackTea" << endl;
      cout << "10. Complete Order" << "\n\n\n";

    
    menuOption();         //calling function menuOption

   
    totalCost(reciept);      //calling function total
                               // to make whole program run again 
      CustomerReciept();        //calling function CustomerReciept to write reciept to file

      // Ask user if they would like the program to repeat for a new order
    cout << "\n\nWould you like to enter new order? Enter (Y/N):";
    cin >> again;


    
     
      //if the input to repeat the program is y, the program will repeat
      //if the input is n, the program will end
      //if the input is neither y or n, the program will ask the user to input y or n again
      while (again=='N'||again=='n')
        {
          cout<<"\nThank you for using the Coffee & Tea POS System.\nThe program has ended.*"<<endl;
          return(0);
         }

  while (again!='Y'&&again!='y'&&again!='N'&&again!='n')
    {
    cout<<"Invalid.\nPlease enter (Y/N): ";
    cin>>again;

      if (again =='Y' ||again =='y')
      {
        
      main();
      }
    }
    

    clear();
 
   
  }while (again =='Y' ||again =='y');
  
return 0;
}


////////////////////////////////////////////////////////////////

void menuOption()                   //function header for menuOption
{


 int count;
  cin >>option;

  while (cin.fail())                //input validation. Using cin.fail If letter is                                           //entered by user
  {
    cin.clear();                    //to clear input buffer in order to restore cin to usable state
    cin.ignore(INT_MAX, '\n');      //to ignore last input
    cout << "Invalid.\nPlease enter a number 1-10: ";
    cin >> option;
  }

//If the option for the menu is not numbers 1-10
 while (option <1 || option >10)
 {
  cout << "Invalid.\nPlease enter a number 1-10: ";
  cin >> option;
 }
//If the option for the first item is 10, the program will ask the user
//if they would like to start another order. If the choice is not a "Y" or "N",
//it will detect invalid. If "Y", it will restart program. If "N", it will end.
  if (option == 10)                 
     {
       cout<<"\nCompleting Order...\n\nThere is no order to process.\n";
       cout<<"\nWould you like to start another order? (Y/N): ";
       cin>>again;

       while (again!='Y'&&again!='y'&&again!='N'&&again!='n')
       {
        cout<<"Invalid. Please enter (Y/N): ";
        cin>>again;
        }
        
        if (again =='Y' ||again =='y')
        {
          main();
        }

        if (again == 'N' || again == 'n')
        {
        cout<<"\nThank you for using the Coffee & Tea POS System.\nThe program has ended.*";
        return menuOption();
        }
      }

  do 
    {
    
     //Shows item choice in order. ex) first choice is showing as item #1, second
     //choice is showing as item #2
      cout << "\nItem # " << ((count++)+1) << ": " << items[option-1] << "\n\n";
     
     itemsList.push_back(items[option-1]);  //adding item names to vector itemsList

      vector<string> drinks;
      desiredDrinkSize(drinks);

      cout << "Choose drink number that customer is ordering: \n";
      cout << "-----------------------------------------------" << endl;
      cout << "1.Latte" << endl;
      cout << "2.Cappuccino" << endl;
      cout << "3.Frappuccino" << endl;
      cout << "4.Americano" << endl;
      cout << "5.Iced Coffee" << endl;
      cout << "6.Green Tea" << endl;
      cout << "7.Passion-fruit Tea" << endl;
      cout << "8.Peach Tea" << endl;
      cout << "9.BlackTea" << endl;
      cout << "10. Complete Order" << "\n\n\n";
      cin >> option;
      
      //If input is a letter for choice
      while (cin.fail())
        {
         cin.clear();
          cin.ignore(INT_MAX, '\n');
          cout << "Invalid.\nPlease enter a number 1-10: ";
         cin >> option;
        }
    //If input is not numbers 1-10 for choice
    if (option <1 || option >10)
        {
          cout << "Invalid.\nPlease enter a number 1-10: ";
          cin >> option;
         }
             

  }
  while (option >=1 && option <=9);    // while loop to add more items on list

  //If choice is 10, the order will process and return to the totalCost function
  if (option == 10)
     {
       cout<<"\nCompleting order...\n";
       return;}

 }     
  
/////////////////////////////////////////////////////////////////


void desiredDrinkSize(vector<string>drinks)     //function header for desiredDrinkSize
{
  //Adding sizes to the vector.
  drinks.push_back("Small");
  drinks.push_back("Medium");
  drinks.push_back("Large");

  //for (int index=0; index <drinks.size(); index++) // displays sizes
  //cout << drinks[index] << endl;

  //Asking user for drink size   
  cout << "Please enter size of drink(S/M/L): " ;
  cin >> drinkSize;
      
  //for (int index=0; index <reciept.size(); index++)  //displays all drinks ordered
  //cout << reciept[] <<endl;
  
  //If input is not a valid size, it will ask same question 
  while (drinkSize != 's'&& drinkSize != 'S' && drinkSize != 'm' && drinkSize != 'M' && drinkSize != 'L' && drinkSize != 'l')
  {
  cout << "Invalid. Please enter size(S/M/L): " << " ";
  cin >> drinkSize;
  }
      //When the sizes are valid, it will store each case 
      while (drinkSize == 's'|| drinkSize == 'S' || drinkSize == 'm' || drinkSize == 'M' || drinkSize == 'L' || drinkSize == 'l')
        {  
          cout << showpoint << fixed << setprecision(2);
         switch (drinkSize)                     //switch to dispay size entered while also adding price to reciept calculator
          {
          case 's' : {cout << "\nSize entered: " << drinks[0] << "\n" <<endl;
                    reciept.push_back(price[0]);
                     cout << "Price: $" << price[0] << "\n\n "<<  endl;
                    
                    }
          break;
          case 'S' : {cout << "\nSize entered: " << drinks[0] << "\n" <<endl;
                    reciept.push_back(price[0]);
                     cout << "Price: $" << price[0] << "\n\n " <<  endl;
                    
                    }
          break;
          case 'm' : {cout << "\nSize entered: " << drinks[1] << "\n" <<endl;
                    reciept.push_back(price[1]);
                     cout << "Price: $" << price[1] << "\n\n " << endl;
                    }
          break;
         case 'M' : {cout << "\nSize entered: " << drinks[1] << "\n" <<endl;
                  reciept.push_back(price[1]);
                     cout << "Price: $" << price[1] << "\n\n " << endl;
                    }
         break;
         case 'l' :  {cout << "\nSize entered: " << drinks[2] << "\n" <<endl;
                  reciept.push_back(price[2]);
                     cout << "Price: $" << price[2] << "\n\n " << endl;
                    }
          break;
         case 'L' : {cout << "\nSize entered: " << drinks[2] << "\n" <<endl;
                  reciept.push_back(price[2]);
                     cout << "Price: $" << price[2] << "\n" << endl;
                    }
          break;
           }       
       return;

      }

}     

//////////////////////////////////////////////////////////////////////

void list(vector<string>itemsList)        //funtion header for list 
{                                         // will show items ordered along with the prices
 for (int index = 0; index <itemsList.size(); index++)
 {
  cout << fixed << left << setw(18);     //for item list alignment
  cout  <<  itemsList[index] <<"\t\t" << "$" << reciept[index] << "\n";
 }

}

/////////////////////////////////////////////////////////////////////


void totalCost(vector<double>reciept)      //function header for totalCost which calculates totals
{
  
 
  cout << "\nItem: \t\t\t\t\t" << "Price:" << endl;
  cout << "----------------------------------\n";

 // for (int count=0; count <reciept.size(); count++) // displays list of prices from items   ordered

  list(itemsList);



  for (int count=0; count <reciept.size(); count++)
  { 
   subTotal += reciept[count];                      //Adding item(s) together
  }
                    
                 
             
    
  cout << showpoint << fixed << setprecision(2);    //for price display
  cout << "\n\nSubtotal: $" << subTotal << endl;

    
  tax= subTotal * taxRate;                          //Calculating tax to the subtotal
  cout << "Tax: $" << tax <<endl;
  total = (subTotal * taxRate)+ subTotal; 

  cout << "\nTotal: $" << total << endl;            //Displaying total


  cout << "Amount paid: $";                         //How much did the customer pay
  cin >> pay;


  while (pay < total)                 //If the pay is less than the total, it is invalid                                        //until an acceptable input is entered
    {
         cout<<"\nInvalid payment.\nPlease input a valid payment amount: $";
    cin>> pay;
    cout<<"\n";}
    
  if (pay==total)
    {
       change = pay - total;           //Calculating the total minus the pay for change

      cout << "Change due: $" <<  change<< endl; 

      cout << "\nTell "<< customerName <<" thank you & to have a nice day!" << endl;
  
    }

    change = pay - total;           //Calculating the total minus the pay for change

    cout << "Change due: $" <<  change<< endl; 

    cout << "\nTell "<< customerName <<" thank you & to have a nice day!" << endl;
 
    
}


////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////

//To output the reciept and store info onto "Reciept.txt"
void CustomerReciept()     //function header for CustomerReciept included to write out to                                 //"Reciept.txt"
{
  fstream dataFile;
  dataFile.open("Reciept.txt", ios::out);
  
  dataFile << "\t\t\t\t\t\tCoffee & Tea" << endl;
  dataFile << "\t\t\t\t********************" << endl;
  dataFile << "5613 Second St. Moreno Valley CA 92555" << endl;
  dataFile << "Tel: 951-555-1563";
  dataFile << " " << endl;
  dataFile << " " << endl;

  dataFile << "Customer's name: ";
  dataFile << customerName;


  dataFile << " " << endl;
  dataFile << " " << endl;
   
  dataFile << "\nItem: \t\t\t\t\t" << "Price:" << endl;
  dataFile << "----------------------------------\n";

 // for (int count=0; count <reciept.size(); count++) // displays list of prices from items   ordered

  for (int index = 0; index <itemsList.size(); index++)
    {
    dataFile << fixed << left << setw(18);
    dataFile << showpoint << fixed << setprecision(2);
    dataFile  <<  itemsList[index] <<"\t\t" << "$" << reciept[index] << "\n";
    }
  
   
    dataFile << showpoint << fixed << setprecision(2);
    dataFile << "\n\nSubtotal: $" << subTotal << endl;
    dataFile << "Tax: $" << tax <<endl;
    dataFile << "\nTotal: $" << total << endl;   


    dataFile << "Amount paid: $";

    
    dataFile << pay << endl;
      if(pay>total)
     

    dataFile << "Change due: $" <<  change << endl; 
    dataFile << "----------------------------------\n";
    dataFile << "\nThank you! Have a nice day!" <<endl;
    dataFile.close();



}


void clear()
{

 // cout << "Reciept vector has " << reciept.size() << " elements" << endl;
      //When the program repeats the vectors will be clear of all previous elements
 

    reciept.resize(0);
    

 // cout << "Reciept vector  now has " << reciept.size() << " elements" << endl;

 // cout << "itemList vector has " << itemsList.size() << " elements" << endl;
  itemsList.clear();

  itemsList.resize(0);
//  cout << "itemList vector has " << itemsList.size() << " elements" << endl;
}
