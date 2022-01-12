#include <iostream>
#include <utility>
#include <string>
#include <sstream>
#include <ctype.h>
#include <compare>
#include <tuple>
using namespace std;

float invoice(long double amount, float bill) {
	
	float change = amount - bill;
	cout << "\nAmount entered: " << amount << endl;
	cout << "Total Bill: " << bill << endl;
	
	return change;
}
void menu(pair<string, float> coffee_menu[], pair<string, float> tea_menu[], int drink_type[]) {

	cout << "\n\nCoffee Menu includes:\n" << endl;
	cout << "Drinks:\t\tPrice   :(AED)\n" << endl;

	for (int i = 0; i < 3; i++) {

		cout << drink_type[i] << ". " << coffee_menu[i].first << "\t\t: " << coffee_menu[i].second << endl;
	}

	cout << "\n\nTea Menu includes:\n" << endl;
	cout << "Drinks:\t\tPrice   :(AED)\n" << endl;

	for (int i = 0; i < 3; i++) {		
		 
		cout << drink_type[i] << ". " << tea_menu[i].first << "\t\t: " << tea_menu[i].second << endl; 
	}
}
struct Drink {  // creating a drink structure
	
	string sugar;
	string type;
	string name;
	long double price;	

	Drink(string t, string n, long double p, string s) {  // costructor for drink

		type = t;
		name = n;
		price = p;
		sugar = s;
	}
};
int main() {

	int drink_type[] = { 1, 2, 3 };
	string coffee_options[] = { "Ice Coffee", "Milk Coffee", "Black Coffee" };
	float coffee_prices[] = { 3.0, 2.0, 1.0 };
	
	string type, name; long double price{}; string sugar{};
	string tea_options[] = { "Ice Tea", "Milk Tea", "Black Tea" };
	float tea_prices[] = { 3.0, 2.0, 1.0 };
	
	pair<string, float> coffee_menu[3];
	pair<string, float> tea_menu[3];

	for (int i = 0; i < 3; i++) {

		coffee_menu[i] = make_pair(coffee_options[i], coffee_prices[i]);
		tea_menu[i] = make_pair(tea_options[i], tea_prices[i]);
	}
	cout << "\t\tWelcome to Lance's Tea and CoffeeShop \n \n";
	int option;

	cout << "Enter 1 for menu, 2 to exit: ";
	cin >> option;
	while (cin.fail() || option != 1 && option != 2) {

		cin.clear();
		cin.ignore(1000, '\n');
		cout << "\nInvalid input, enter a valid option (1/2) :\n\n";		
		cin >> option;
	}
	if (option == 2) { cout << "\n\t\t\tThanks for visiting\n" << endl; return 0; }
	menu(coffee_menu, tea_menu, drink_type);

	int i = 0; int choice; int drink_name; int sucre; float aed;
	Drink d1(type, name, price, sugar);

	while (true) { // Single while loop for errror handling instead of making individual while loops for each user input

		cin.clear();
		cin.ignore(1000, '\n');
		if (i == 0) {
			cout << "\n\t\tWhat drink would you like? Coffee or Tea? Input 1 for Coffee or 2 for Tea" << endl;
			cin >> choice;
			if (choice == 1) { d1.type = "Coffee"; i = 1; }
			else if (choice == 2) { d1.type = "Tea"; i = 1; }
			else { cout << "\n\nInvalid Drink try again...\n"; }
		}
		if (i == 1) { 
			cout << "\nWhat kind of " << d1.type << " do you want? Input the appropriate number for your drink type (Number Shown before the drink name)\n" << endl;
			cin >> drink_name;
			switch (drink_name) {
			case 1:
				d1.name = "Iced "; i = 2; aed = 3;
				break;
			case 2:
				d1.name = "Milk "; i = 2; aed = 2;
				break;
			case 3:
				d1.name = "Black "; i = 2; aed = 1;
				break;
			default:
				cout << "\n\nInvalid Drink Type, Try again...\n";
				i = 1; break;
			}
		}
		if (i == 2 ) {
			cout << "\nWould you want some sugar with your drink?(Y/N) : ";
			cin >> d1.sugar;
			if (d1.sugar == "Y" || d1.sugar == "y") {

				cout << "\nHow many packets of sugar do you want (max 10) : ";
				cin >> sucre;
				while (cin.fail() || sucre > 10) {
				cin.clear(); cin.ignore(1000, '\n');
				cout << "\nInvalid Sugar amount, Try again... How many packets of sugar do you want: "; cin >> sucre;
				}
				i = 3;
			}
			else if (d1.sugar == "N" || d1.sugar == "n") { i = 3; sucre = 0; }
			else { cout << "\nInvalid response try again...\n"; }
		}
		if (i == 3) {
			cout << "\nPrice of order is " << aed << " AED" << "\n\nEnter Money : ";
			cin >> d1.price;
			if (cin.fail()) { cout << "\nInvalid Input, Try again...\n"; }
			else if (d1.price < aed) { cout << "\nInsufficient funds, Try again\n"; }
			else break; 
		}
	}
	cout << "\nYour order is " << d1.name << d1.type << " - Sugar: " << sucre << endl;
	int change = invoice(d1.price, aed);
	cout << "Change: " << change << endl;
	return 0;
}
