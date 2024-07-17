class TreeNode {
    int key;
    TreeNode left, right;

    public TreeNode(int item) {
        key = item;
        left = right = null;
    }
}

public class BST {
    TreeNode root;

    public BST() {
        root = null;
    }

    // Search
    public TreeNode search(int key) {
        return searchHelper(root, key);
    }

    private TreeNode searchHelper(TreeNode root, int key) {
        if (root == null || root.key == key)
            return root;

        if (key < root.key)
            return searchHelper(root.left, key);

        return searchHelper(root.right, key);
    }

    // Insert
    public void insert(int key) {
        root = insertHelper(root, key);
    }

    private TreeNode insertHelper(TreeNode root, int key) {
        if (root == null) {
            root = new TreeNode(key);
            return root;
        }

        if (key < root.key)
            root.left = insertHelper(root.left, key);
        else if (key > root.key)
            root.right = insertHelper(root.right, key);

        return root;
    }

    // Delete
    public void delete(int key) {
        root = deleteHelper(root, key);
    }

    private TreeNode deleteHelper(TreeNode root, int key) {
        if (root == null)
            return root;

        if (key < root.key)
            root.left = deleteHelper(root.left, key);
        else if (key > root.key)
            root.right = deleteHelper(root.right, key);
        else {
            // Node to be deleted found

            // Case 1: No child or only one child
            if (root.left == null)
                return root.right;
            else if (root.right == null)
                return root.left;

            // Case 2: Node has two children
            root.key = minValue(root.right);

            // Delete the inorder successor
            root.right = deleteHelper(root.right, root.key);
        }

        return root;
    }

    private int minValue(TreeNode root) {
        int minv = root.key;
        while (root.left != null) {
            minv = root.left.key;
            root = root.left;
        }
        return minv;
    }

    // Inorder traversal
    public void inorderTraversal() {
        inorderTraversalHelper(root);
    }

    private void inorderTraversalHelper(TreeNode root) {
        if (root != null) {
            inorderTraversalHelper(root.left);
            System.out.print(root.key + " ");
            inorderTraversalHelper(root.right);
        }
    }

    public static void main(String[] args) {
        BST bst = new BST();
        bst.insert(50);
        bst.insert(30);
        bst.insert(20);
        bst.insert(40);
        bst.insert(70);
        bst.insert(60);
        bst.insert(80);

        System.out.println("Inorder traversal of BST:");
        bst.inorderTraversal();
        System.out.println();

        int keyToSearch = 40;
        TreeNode foundNode = bst.search(keyToSearch);
        if (foundNode != null)
            System.out.println(keyToSearch + " found in the BST.");
        else
            System.out.println(keyToSearch + " not found in the BST.");

        int keyToDelete = 30;
        bst.delete(keyToDelete);
        System.out.println("Deleted node with key " + keyToDelete + ".");

        System.out.println("Inorder traversal after deletion:");
        bst.inorderTraversal();
    }
}
