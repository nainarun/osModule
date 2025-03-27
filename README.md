# osModule
#the assigned module
import os
import shutil

# --- Module 1: Basic File Management ---
class FileManager:
    def __init__(self, node_directory):
        self.node_directory = node_directory
        if not os.path.exists(self.node_directory):
            os.makedirs(self.node_directory)

             def create_file(self, filename, content=""):
        filepath = os.path.join(self.node_directory, filename)
        with open(filepath, 'w') as f:
            f.write(content)
        print(f"[{self.node_directory}] File '{filename}' created.")
def read_file(self, filename):
        filepath = os.path.join(self.node_directory, filename)
        try:
            with open(filepath, 'r') as f:
                return f.read()
        except FileNotFoundError:
            print(f"[{self.node_directory}] File '{filename}' not found.")
            return None
            def update_file(self, filename, new_content):
        filepath = os.path.join(self.node_directory, filename)
        if os.path.exists(filepath):
            with open(filepath, 'a') as f:
                f.write(new_content)
            print(f"[{self.node_directory}] File '{filename}' updated.")
        else:
            print(f"[{self.node_directory}] File '{filename}' does not exist.")
            def delete_file(self, filename):
        filepath = os.path.join(self.node_directory, filename)
        if os.path.exists(filepath):
            os.remove(filepath)
            print(f"[{self.node_directory}] File '{filename}' deleted.")
        else:
            print(f"[{self.node_directory}] File '{filename}' not found.")
            def list_files(self):
        files = [f for f in os.listdir(self.node_directory)
                 if os.path.isfile(os.path.join(self.node_directory, f))]
        print(f"Files in '{self.node_directory}': {files}")
        return files

        class FaultToleranceManager:
    def __init__(self, node_directories):
        # Initialize a FileManager for each active node
        self.nodes = [FileManager(node) for node in node_directories]
        # Keep track of all original node names for recovery purposes
        self.all_node_names = list(node_directories)
    
    def create_file(self, filename, content=""):
        for fm in self.nodes:
            fm.create_file(filename, content)
    
    def update_file(self, filename, new_content):
        for fm in self.nodes:
            fm.update_file(filename, new_content)
    
    def delete_file(self, filename):
        for fm in self.nodes:
            fm.delete_file(filename)
    
    def list_files(self, node_index=0):
        if 0 <= node_index < len(self.nodes):
            return self.nodes[node_index].list_files()
        else:
            print("Invalid node index.")
            return []
    
    def simulate_node_failure(self, node_index):
        if 0 <= node_index < len(self.nodes):
            failed_node_dir = self.nodes[node_index].node_directory
            if os.path.exists(failed_node_dir):
                shutil.rmtree(failed_node_dir)
                print(f"Simulated failure: Node '{failed_node_dir}' is now down.")
            else:
                print(f"Node '{failed_node_dir}' already down.")
            del self.nodes[node_index]
        else:
            print("Invalid node index.")
    
    def recover_node(self, node_directory):
        # Recreate the failed node's directory and copy data from a healthy node (if available)
        if not os.path.exists(node_directory):
            os.makedirs(node_directory)
        if self.nodes:
            source_dir = self.nodes[0].node_directory
            for item in os.listdir(source_dir):
                src = os.path.join(source_dir, item)
                dst = os.path.join(node_directory, item)
                if os.path.isfile(src):
                    shutil.copy(src, dst)
            print(f"Recovered node '{node_directory}' using data from '{source_dir}'.")
            self.nodes.append(FileManager(node_directory))
        else:
            print("No healthy nodes available for recovery.")
    
    def show_active_nodes(self):
        active = [fm.node_directory for fm in self.nodes]
        print(f"Active nodes: {active}")
        return active

# --- Integrated Interactive Interface ---
def interactive_fault_tolerance_manager():
    # Define initial nodes (directories)
    initial_nodes = ["node1", "node2"]
    ftm = FaultToleranceManager(initial_nodes)
    
    menu = """
    Options:
    1: Create file (replicated)
    2: Read file from a node
    3: Update file (replicated)
    4: Delete file (replicated)
    5: List files in a node
    6: Show active nodes
    7: Simulate node failure
    8: Recover a failed node
    9: Exit
    """
    
    while True:
        print(menu)
        choice = input("Enter your choice (1-9): ")
        
        if choice == "1":
            filename = input("Enter filename to create: ")
            content = input("Enter content for the file: ")
            ftm.create_file(filename, content)
        elif choice == "2":
            try:
                node_index = int(input("Enter node index to read from (starting at 0): "))
                filename = input("Enter filename to read: ")
                # Directly create a temporary FileManager instance for reading
                active_nodes = ftm.show_active_nodes()
                if node_index < len(active_nodes):
                    fm = FileManager(active_nodes[node_index])
                    file_content = fm.read_file(filename)
                    if file_content is not None:
                        print(f"Content: {file_content}")
                else:
                    print("Invalid node index.")
            except ValueError:
                print("Invalid input.")
        elif choice == "3":
            filename = input("Enter filename to update: ")
            new_content = input("Enter content to append: ")
            ftm.update_file(filename, new_content)
        elif choice == "4":
            filename = input("Enter filename to delete: ")
            ftm.delete_file(filename)
        elif choice == "5":
            try:
                node_index = int(input("Enter node index to list files (starting at 0): "))
                ftm.list_files(node_index)
            except ValueError:
                print("Invalid input.")
        elif choice == "6":
            ftm.show_active_nodes()
        elif choice == "7":
            try:
                node_index = int(input("Enter node index to simulate failure (starting at 0): "))
                ftm.simulate_node_failure(node_index)
            except ValueError:
                print("Invalid input.")
        elif choice == "8":
            # Recover by providing the node directory name to recover (must be one of the original nodes)
            print("Original node names: ", ftm.all_node_names)
            node_dir = input("Enter the directory name to recover: ")
            if node_dir in ftm.all_node_names:
                ftm.recover_node(node_dir)
            else:
                print("Invalid node directory.")
        elif choice == "9":
            print("Exiting interactive session.")
            break
        else:
            print("Invalid choice. Please select a valid option.")

# Uncomment the next line to run the interactive session:
interactive_fault_tolerance_manager()
