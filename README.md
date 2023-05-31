# 6d-Cistercian-Lattice-DAG-Hexadecimal-string-of-unicode-symbols
The encryptMessage function encrypts a given message by mapping each character to a corresponding symbol from the lattice. The resulting encrypted message is then converted to a string of Unicode code points using the unicodeToString
//

#include <iostream>
#include <vector>
#include <random>
#include <sstream>
#include <iomanip>

// Structure to represent a lattice symbol with Unicode symbol and complexity
struct LatticeSymbol {
    unsigned int symbol;        // Unicode symbol
    unsigned int complexity;    // Complexity
};

// DAG node structure
struct DAGNode {
    std::vector<int> parents;   // Parent indices
    LatticeSymbol symbol;       // Lattice symbol
};

// Function to create a 6D lattice with Unicode symbols
std::vector<DAGNode> createLattice(int width, int height, int depth, int time, int energy, int spirit) {
    // Create a random number generator
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<unsigned int> distribution(0, 114111); // Maximum Unicode code point

    // Create the lattice structure with Unicode symbols
    std::vector<DAGNode> lattice(width * height * depth * time * energy * spirit);

    // Fill the lattice with random Unicode symbols
    for (int i = 0; i < lattice.size(); i++) {
        lattice[i].symbol.symbol = distribution(gen);
        lattice[i].symbol.complexity = gen() % 100; // Random complexity between 0 and 99
    }

    // Connect the lattice nodes based on adjacency
    int numNodes = width * height * depth * time * energy * spirit;
    for (int i = 0; i < numNodes; i++) {
        int x = i % width;
        int y = (i / width) % height;
        int z = (i / (width * height)) % depth;
        int t = (i / (width * height * depth)) % time;
        int e = (i / (width * height * depth * time)) % energy;
        int s = i / (width * height * depth * time * energy);

        if (x > 0) {
            int neighborIndex = i - 1;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (x < width - 1) {
            int neighborIndex = i + 1;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (y > 0) {
            int neighborIndex = i - width;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (y < height - 1) {
            int neighborIndex = i + width;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (z > 0) {
            int neighborIndex = i - width * height;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (z < depth - 1) {
            int neighborIndex = i + width * height;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (t > 0) {
            int neighborIndex = i - width * height * depth;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (t < time - 1) {
            int neighborIndex = i + width * height * depth;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (e > 0) {
            int neighborIndex = i - width * height * depth * time;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (e < energy - 1) {
            int neighborIndex = i + width * height * depth * time;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (s > 0) {
            int neighborIndex = i - width * height * depth * time * energy;
            lattice[i].parents.push_back(neighborIndex);
        }
        if (s < spirit - 1) {
            int neighborIndex = i + width * height * depth * time * energy;
            lattice[i].parents.push_back(neighborIndex);
        }
    }

    return lattice;
}

// Function to encrypt a message using the 6D lattice symbols
std::vector<unsigned int> encryptMessage(const std::string& message, const std::vector<DAGNode>& lattice) {
    std::vector<unsigned int> encryptedData;

    for (char c : message) {
        unsigned int latticeIndex = c % lattice.size();
        unsigned int symbol = lattice[latticeIndex].symbol.symbol;
        encryptedData.push_back(symbol);
    }

    return encryptedData;
}

// Function to convert a list of Unicode code points to a string
std::string unicodeToString(const std::vector<unsigned int>& codePoints) {
    std::ostringstream oss;
    for (unsigned int codePoint : codePoints) {
        oss << std::hex << std::setw(4) << std::setfill('0') << codePoint;
    }
    return oss.str();
}

int main() {
    // Create a 6D lattice with Unicode symbols
    int width = 10;
    int height = 10;
    int depth = 10;
    int time = 10;
    int energy = 10;
    int spirit = 10;
    std::vector<DAGNode> lattice = createLattice(width, height, depth, time, energy, spirit);

    // Encrypt a message using the 6D lattice symbols
    std::string message = "I pr41se tH3 L0Rd 4nd Br34k th3 L4w!";
    std::vector<unsigned int> encryptedMessage = encryptMessage(message, lattice);

    // Output the encrypted message as a string of Unicode code points
    std::string encryptedString = unicodeToString(encryptedMessage);
    std::cout << "Encrypted Message (Unicode Code Points): " << encryptedString << std::endl;

    return 0;
}
