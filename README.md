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
 
