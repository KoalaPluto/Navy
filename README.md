#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <cctype>

using namespace std;

// Fungsi untuk mengubah input menjadi huruf besar (case insensitive)
string toUpper(const string& str) {
    string upperStr = str;
    transform(upperStr.begin(), upperStr.end(), upperStr.begin(), ::toupper);
    return upperStr;
}

// Struct untuk karakter Employee (pegawai)
struct Employee {
    string name;
    int energy;
    int loves;

    Employee(string n, int e, int l) : name(n), energy(e), loves(l) {}
    
    void serve() {
        if (energy > 0) {
            energy -= 10;
            loves += 5;
            cout << name << " served a customer. Energy: " << energy << ", Loves: " << loves << endl;
        } else {
            cout << name << " is too tired to serve. Needs rest." << endl;
        }
    }

    void rest() {
        energy += 20;
        cout << name << " is resting. Energy: " << energy << endl;
    }
};

// Struct untuk karakter Pet (hewan peliharaan)
struct Pet {
    string name;
    int energy;
    int loves;

    Pet(string n, int e, int l) : name(n), energy(e), loves(l) {}
    
    void petting() {
        loves += 10;
        cout << name << " is being petted. Loves: " << loves << endl;
    }

    void feed() {
        energy += 15;
        cout << name << " is being fed. Energy: " << energy << endl;
    }
};

// Deklarasi vektor untuk menyimpan data karakter Employee dan Pet
vector<Employee> employees = { Employee("Diane", 60, 50), Employee("Alice", 70, 40), Employee("Bob", 80, 30) };
vector<Pet> pets = { Pet("Caty", 50, 60), Pet("Buddy", 45, 55), Pet("Max", 55, 65) };

// Fungsi mencari karakter Employee berdasarkan nama
Employee* findEmployee(const string& name) {
    for (auto& emp : employees) {
        if (toUpper(emp.name) == toUpper(name)) return &emp;
    }
    return nullptr;
}

// Fungsi mencari karakter Pet berdasarkan nama
Pet* findPet(const string& name) {
    for (auto& pet : pets) {
        if (toUpper(pet.name) == toUpper(name)) return &pet;
    }
    return nullptr;
}

// Fungsi untuk menampilkan atribut karakter berdasarkan nama
void displayAttributes(const string& name) {
    Employee* emp = findEmployee(name);
    if (emp) {
        cout << "Employee " << emp->name << " - Energy: " << emp->energy << ", Loves: " << emp->loves << endl;
        return;
    }
    Pet* pet = findPet(name);
    if (pet) {
        cout << "Pet " << pet->name << " - Energy: " << pet->energy << ", Loves: " << pet->loves << endl;
        return;
    }
    cout << "Character " << name << " not found." << endl;
}

// Fungsi untuk menampilkan urutan karakter berdasarkan atribut loves
void showSortedByLoves(const string& type) {
    if (type == "EMPLOYEE") {
        sort(employees.begin(), employees.end(), [](const Employee& a, const Employee& b) {
            return a.loves > b.loves;
        });
        cout << "Employees sorted by loves:" << endl;
        for (const auto& emp : employees) {
            cout << emp.name << " - Loves: " << emp.loves << endl;
        }
    } else if (type == "PET") {
        sort(pets.begin(), pets.end(), [](const Pet& a, const Pet& b) {
            return a.loves > b.loves;
        });
        cout << "Pets sorted by loves:" << endl;
        for (const auto& pet : pets) {
            cout << pet.name << " - Loves: " << pet.loves << endl;
        }
    } else {
        cout << "Invalid type for sorting. Please use 'EMPLOYEE' or 'PET'." << endl;
    }
}

// Fungsi untuk memproses perintah dari pengguna
void processCommand(const string& command) {
    string action, name;
    size_t spacePos = command.find(' ');
    if (spacePos != string::npos) {
        action = command.substr(0, spacePos);
        name = command.substr(spacePos + 1);
    } else {
        action = command;
    }

    action = toUpper(action);

    if (action == "PETTING") {
        Pet* pet = findPet(name);
        if (pet) pet->petting();
        else cout << "Pet " << name << " not found." << endl;
    } else if (action == "FEED") {
        Pet* pet = findPet(name);
        if (pet) pet->feed();
        else cout << "Pet " << name << " not found." << endl;
    } else if (action == "SERVE") {
        Employee* emp = findEmployee(name);
        if (emp) emp->serve();
        else cout << "Employee " << name << " not found." << endl;
    } else if (action == "REST") {
        Employee* emp = findEmployee(name);
        if (emp) emp->rest();
        else cout << "Employee " << name << " not found." << endl;
    } else if (action == "ATTR") {
        displayAttributes(name);
    } else if (action == "SHOW") {
        string type = toUpper(name);
        if (type == "EMPLOYEE LOVES") showSortedByLoves("EMPLOYEE");
        else if (type == "PET LOVES") showSortedByLoves("PET");
        else cout << "Invalid SHOW command format. Use 'SHOW EMPLOYEE LOVES' or 'SHOW PET LOVES'." << endl;
    } else {
        cout << "Unknown command. Please try again." << endl;
    }
}

// Fungsi utama untuk menjalankan simulasi
int main() {
    string command;
    cout << "Welcome to the Pet Cafe simulation! Type a command or type 'EXIT' to quit:" << endl;

    while (true) {
        cout << "> ";
        getline(cin, command);

        if (toUpper(command) == "EXIT") break;

        processCommand(command);
    }

    cout << "Exiting Pet Cafe simulation. Goodbye!" << endl;
    return 0;
}
