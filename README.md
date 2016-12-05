#include <iostream>
#include <string>

/*
This is a group work by
Ani Gevorgyan &
Anahit Amirkhanyan
*/

using namespace std;

template <class T>
struct node
{
	T data;
	struct node *left;
	struct node *right;
};

template <class T>
class BinarySearchTree
{
	node <T> *root;
	
	BinarySearchTree()
	{
		root = NULL;
	}

	~BinarySearchTree()
	{
		destroyTree(root);
	}

	void destroyTree( node <T> *root)
	{
		if (root != NULL)
		{
			destroyTree(root->left);
			destroyTree(root->right);
			delete root;
		}
	}

	void insert(T data)
	{
		if (root == NULL)
		{
			root = new node <T>;
			root->data = data;
			root->left = NULL;
			root->right = NULL;
		}

		else insertHelper(root, data);
	}

	void insertHelper(node <T> *leaf, T data)
	{
		if (data < leaf->data)
		{
			if (leaf->left == NULL)
			{
				leaf->left = new node <T>;
				leaf->left->data = data;
				leaf->left->left = NULL;
				leaf->left->right = NULL;
			}

			else insertHelper(leaf->left, data); 
		}

		else
		{
			if (leaf->right == NULL)
			{
				leaf->right = new node <T>;
				leaf->right->data = data;
				leaf->right->left = NULL;
				leaf->right->right = NULL;
			}

			else insertHelper(leaf->right, data);
		}
	}

	bool find(T data)
	{
		if (findHelper(data, root) == NULL)
			return false;
		return true;
	}


	node<T> *findHelper(T data, node<T> *leaf)
	{
		if (leaf != NULL) 
		{
			if (data == leaf->data)
				return leaf;
			if (data < leaf->data)
				return findHelper(data, leaf->left);
			else
				return findHelper(data, leaf->right);
		}
		else return NULL;
	}

	node<T>* remove_value(node<T> *leaf, int data)
	{
		if (leaf == NULL) 
			return leaf;
		else if (data < leaf->data)
			leaf->left = remove_value(leaf->left, data);
		else if (data > leaf->data)
			leaf->right = remove_value(leaf->right, data);
		else {
			if (leaf->left == NULL && leaf->right == NULL)
			{
				delete leaf; 
				leaf = NULL;
			}
			else if (leaf->left == NULL) 
			{
				node<T> *temp = leaf;
				leaf = leaf->right;
				delete temp;
			}
			else if (leaf->right == NULL) 
			{
				node <T> *temp = leaf;
				leaf = leaf->left;
				delete temp;
			}
			else 
			{
				node<T> *temp = leaf->right;
				while (temp->left != NULL) 
					temp = temp->left;
				leaf->data = temp->data;
				leaf->right = remove_value(leaf->right, temp->data);
			}
		}
		return leaf;
	}

	void inorder_traverse(node <T> *leaf)
	{
		if (leaf != NULL)
		{
			inorder_traverse(leaf->left);
			cout << leaf->data << "  ";
			inorder_traverse(leaf->right);
		}
	}

	void preorder_traverse(node <T> *leaf)
	{
		if (leaf != NULL)
		{
			cout << leaf->data << "  ";
			preorder_traverse(leaf->left);
			preorder_traverse(leaf->right);
		}
	}

	void postorder_traverse(node <T> *leaf)
	{
		if (leaf != NULL)
		{
			postorder_traverse(leaf->left);
			postorder_traverse(leaf->right);
			cout << leaf->data << "  ";
		}
	}

	int get_height(node <T> *leaf)
	{
		if (leaf == NULL)
			return 0;
		else
			return 1 + max(get_height(leaf->left), get_height(leaf->right));
	}

};
