#include <iostream>
using namespace std;

// Node structure
struct Node {
    int key;
    Node* left;
    Node* right;
Node(int k) : key(k), left(nullptr), right(nullptr) {}
};

// --------- BST OPERATIONS ---------

// Search key in BST (recursive)
Node* search(Node* root, int key) {
    if (root == nullptr || root->key == key)
        return root;
   if (key < root->key)
        return search(root->left, key);
    else
        return search(root->right, key);
}

// Insert key into BST (recursive)
Node* insert(Node* root, int key) {
    if (root == nullptr) {
        return new Node(key);
    }

  if (key < root->key)
        root->left = insert(root->left, key);
    else if (key > root->key)
        root->right = insert(root->right, key);
    // if key == root->key, we do nothing (no duplicates)
    return root;
}

// Find minimum value node in a (sub)tree
Node* findMin(Node* root) {
    while (root && root->left != nullptr)
        root = root->left;
    return root;
}

// Delete key from BST (recursive)
Node* deleteNode(Node* root, int key) {
if (root == nullptr) return root;

if (key < root->key) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->key) {
        root->right = deleteNode(root->right, key);
    } else {
        // node to be deleted found

// Case 1: no child or only right child
        if (root->left == nullptr) {
            Node* temp = root->right;
            delete root;
            return temp;
        }
        // Case 2: only left child
        else if (root->right == nullptr) {
            Node* temp = root->left;
            delete root;
            return temp;
        }

 // Case 3: two children
        Node* temp = findMin(root->right);
        root->key = temp->key; // copy value
        // Delete the inorder successor
        root->right = deleteNode(root->right, temp->key);
    }
    return root;
}

// Inorder traversal (sorted order)
void inorder(Node* root) {
    if (root == nullptr) return;
    inorder(root->left);
    cout << root->key << " ";
    inorder(root->right);
}

int main() {
    Node* root = nullptr;

// Insert some values
    int values[] = {50, 30, 70, 20, 40, 60, 80};
    for (int x : values) {
        root = insert(root, x);
    }

cout << "Inorder traversal (initial): ";
    inorder(root);
    cout << endl;

// Search for a key
    int keyToSearch = 40;
    Node* result = search(root, keyToSearch);
    if (result != nullptr)
        cout << "Found key " << keyToSearch << endl;
    else
        cout << "Key " << keyToSearch << " not found" << endl;

// Delete a key
    int keyToDelete = 30;
    root = deleteNode(root, keyToDelete);
    cout << "Inorder traversal after deleting " << keyToDelete << ": ";
    inorder(root);
    cout << endl;

  return 0;
}
