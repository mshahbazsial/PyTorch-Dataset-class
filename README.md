import torch
from torch.utils.data import Dataset
import os

class NameDataset(Dataset):
    def __init__(self, data_dir):
        self.names = []
        self.labels = []
        self.abc = "ABCDEFGHIJKLMNOPQRSTUVWXYZ" + "ABCDEFGHIJKLMNOPQRSTUVWXYZ".lower() + "_"
        
        self.country_map = {
            'Arabic': 0, 'Chinese': 1, 'Czech': 2, 'Dutch': 3,
            'English': 4, 'French': 5, 'German': 6, 'Greek': 7,
            'Irish': 8, 'Italian': 9, 'Japanese': 10, 'Korean': 11,
            'Polish': 12, 'Portuguese': 13, 'Russian': 14, 'Scottish': 15,
            'Spanish': 16, 'Vietnamese': 17
        }
        
        for filename in os.listdir(data_dir):
            if filename.endswith('.txt'):
                country = filename.replace('.txt', '')
                if country in self.country_map:
                    with open(os.path.join(data_dir, filename), 'r', encoding='utf-8') as f:
                        for line in f:
                            name = line.strip()
                            if name:
                                self.names.append(name)
                                self.labels.append(self.country_map[country])
    
    def __len__(self):
        return len(self.names)
    
    def letter_index(self, lett):
        if lett in self.abc:
            return self.abc.find(lett)
        return self.abc.find("_")
    
    def __getitem__(self, idx):
        name = self.names[idx]
        label = self.labels[idx]
        
        name_tensor = torch.zeros(len(name), len(self.abc))
        for index, lett in enumerate(name):
            name_tensor[index][self.letter_index(lett)] = 1
        
        country_tensor = torch.zeros(18)
        country_tensor[label] = 1
        
        return name_tensor, country_tensor

or.shape)
    # Your training code here
