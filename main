#include <iostream>
#include <unordered_map>
#include <map>
#include <memory>
#include <stdexcept>
#include <string>

// Custom exception classes
class DuplicateItemException : public std::runtime_error {
public:
    DuplicateItemException(const std::string& msg) : std::runtime_error(msg) {}
};

class ItemNotFoundException : public std::runtime_error {
public:
    ItemNotFoundException(const std::string& msg) : std::runtime_error(msg) {}
};

// StoredItem class definition
class StoredItem {
private:
    std::string id;
    std::string description;
    std::string location;

public:
    StoredItem(std::string id, std::string desc, std::string loc)
        : id(id), description(desc), location(loc) {}

    std::string getId() const { return id; }
    std::string getDescription() const { return description; }
    std::string getLocation() const { return location; }
};

// StorageManager class definition
class StorageManager {
private:
    std::unordered_map<std::string, std::shared_ptr<StoredItem>> itemById;
    std::map<std::string, std::shared_ptr<StoredItem>> itemByDescription;

public:
    void addItem(const std::shared_ptr<StoredItem>& item) {
        if (itemById.find(item->getId()) != itemById.end()) {
            throw DuplicateItemException("Item with ID " + item->getId() + " already exists!");
        }
        itemById[item->getId()] = item;
        itemByDescription[item->getDescription()] = item;
    }

    std::shared_ptr<StoredItem> findById(const std::string& id) const {
        auto it = itemById.find(id);
        if (it == itemById.end()) throw ItemNotFoundException("Item with ID " + id + " not found!");
        return it->second;
    }

    void removeItem(const std::string& id) {
        auto it = itemById.find(id);
        if (it == itemById.end()) throw ItemNotFoundException("Item with ID " + id + " not found!");

        itemByDescription.erase(it->second->getDescription());
        itemById.erase(id);
    }

    void listItemsByDescription() const {
        std::cout << "\nItems sorted by description:\n";
        for (const auto& item : itemByDescription) {
            std::cout << "- " << item.second->getDescription() << ": " << item.second->getLocation() << std::endl;
        }
    }
};

// Main function demonstrating the system
int main() {
    try {
        StorageManager manager;

        auto item1 = std::make_shared<StoredItem>("ITEM001", "LED Light", "Aisle 3, Shelf 1");
        auto item2 = std::make_shared<StoredItem>("ITEM002", "Fan Motor", "Aisle 2, Shelf 5");

        std::cout << "Adding item: ITEM001 - LED Light\n";
        manager.addItem(item1);
        std::cout << "Adding item: ITEM002 - Fan Motor\n";
        manager.addItem(item2);

        std::cout << "Attempting to add ITEM001 again...\n";
        try {
            manager.addItem(item1);
        } catch (const std::exception& e) {
            std::cerr << "Error: " << e.what() << std::endl;
        }

        std::cout << "Retrieving ITEM002...\n";
        std::shared_ptr<StoredItem> foundItem = manager.findById("ITEM002");
        std::cout << "Found: " << foundItem->getDescription() << " at " << foundItem->getLocation() << "\n";

        std::cout << "Removing ITEM003...\n";
        try {
            manager.removeItem("ITEM003");
        } catch (const std::exception& e) {
            std::cerr << "Error: " << e.what() << std::endl;
        }

        manager.listItemsByDescription();

    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }

    return 0;
}
