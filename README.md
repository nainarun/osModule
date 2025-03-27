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
