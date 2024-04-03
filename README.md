hi
1. HomeVC

    import UIKit

class HomeVC: UIViewController {
    
    // Outlets
    @IBOutlet weak var usernameTF: UITextField!
    @IBOutlet weak var passwordTF: UITextField!
    @IBOutlet weak var signInBTN: UIButton!
    
    // MARK: - View Lifecycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Disable sign in button initially
        signInBTN.isEnabled = false
        
        // Set content type for username and password text fields
        usernameTF.textContentType = .username
        passwordTF.textContentType = .password
        
        // Add target for text fields to enable/disable sign in button
        usernameTF.addTarget(self, action: #selector(textFieldDidChange(_:)), for: .editingChanged)
        passwordTF.addTarget(self, action: #selector(textFieldDidChange(_:)), for: .editingChanged)
    }
    
    // MARK: - Actions
    
    @IBAction func signIn(_ sender: UIButton) {
        // Validate username and password
        if usernameTF.text == "admin" && passwordTF.text == "P@$$w0rd" {
            performSegue(withIdentifier: "segueToActionItems", sender: nil)
        } else {
            // Show alert for invalid credentials
            let alert = UIAlertController(title: "Invalid Credentials", message: "Please enter correct username and password.", preferredStyle: .alert)
            alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
            present(alert, animated: true, completion: nil)
        }
    }
    
    // MARK: - Helper Methods
    
    @objc func textFieldDidChange(_ textField: UITextField) {
        // Enable sign in button only if both username and password are non-empty
        signInBTN.isEnabled = !(usernameTF.text?.isEmpty ?? true) && !(passwordTF.text?.isEmpty ?? true)
    }
}





2. ActionItemsTableVC


 import UIKit

class ActionItemsTableVC: UIViewController, UITableViewDelegate, UITableViewDataSource {
    
    // Outlets
    @IBOutlet weak var tableView: UITableView!
    
    // Data model
    var todoItems = ["Task 1", "Task 2", "Task 3"]
    var archivedItems = ["Task 4", "Task 5"]
    
    // MARK: - View Lifecycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Set delegate and data source for table view
        tableView.delegate = self
        tableView.dataSource = self
        
        // Set navigation item title
        navigationItem.title = "Hello, admin"
        
        // Register custom cell if needed
        // tableView.register(CustomTableViewCell.self, forCellReuseIdentifier: "CustomCell")
    }
    
    // MARK: - Table View Data Source
    
    func numberOfSections(in tableView: UITableView) -> Int {
        return 2 // One section for todo items, one for archived items
    }
    
    func tableView(_ tableView: UITableView, titleForHeaderInSection section: Int) -> String? {
        return section == 0 ? "To Do" : "Archived"
    }
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return section == 0 ? todoItems.count : archivedItems.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        let item = indexPath.section == 0 ? todoItems[indexPath.row] : archivedItems[indexPath.row]
        cell.textLabel?.text = item
        cell.imageView?.image = indexPath.section == 0 ? UIImage(systemName: "arrow.right.circle.fill") : UIImage(systemName: "checkmark.circle.fill")
        return cell
    }
    
    // MARK: - Table View Delegate
    
    func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
        let actionTitle = indexPath.section == 0 ? "Archive" : "Move to To Do"
        let action = UIContextualAction(style: .destructive, title: actionTitle) { (action, view, completionHandler) in
            // Perform action based on section
            if indexPath.section == 0 {
                // Archive action item
                let item = self.todoItems.remove(at: indexPath.row)
                self.archivedItems.append(item)
            } else {
                // Move archived item back to todo list
                let item = self.archivedItems.remove(at: indexPath.row)
                self.todoItems.append(item)
            }
            self.tableView.reloadData()
            completionHandler(true)
        }
        return UISwipeActionsConfiguration(actions: [action])
    }
}



3. Bonus

   override func viewDidLoad() {
    super.viewDidLoad()

    // Step 1: Import the module where ActionItemView and ActionItemViewDelegate are declared
    // Assume ActionItemView is declared in a module named "CustomViews"
    import CustomViews

    // Step 2: Create an instance of ActionItemView
    let actionItemView = ActionItemView()

    // Step 3: Set HomeVC as the delegate of ActionItemView
    actionItemView.delegate = self

    // Step 4: Implement the required methods of ActionItemViewDelegate in HomeVC
    // For example:
    extension HomeVC: ActionItemViewDelegate {
        func actionItemViewDidSomething(_ actionItemView: ActionItemView) {
            // Handle the action in HomeVC
            print("ActionItemView did something!")
        }
    }

    // Step 5: Add ActionItemView to the view hierarchy if necessary
    // Assuming you want to add it as a subview
    view.addSubview(actionItemView)

    // Step 6: Layout and configure ActionItemView as needed
    // Assuming you want to set its frame and other properties
    actionItemView.frame = CGRect(x: 100, y: 100, width: 200, height: 100)

    // Additional configuration if needed

    // Multiline print statement to display the steps
    print("""
    Delegation mechanism setup:
    1. Import CustomViews module
    2. Create an instance of ActionItemView
    3. Set HomeVC as the delegate of ActionItemView
    4. Implement required methods of ActionItemViewDelegate in HomeVC
    5. Add ActionItemView to the view hierarchy
    6. Layout and configure ActionItemView
    """)
}
