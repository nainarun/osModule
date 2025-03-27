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
