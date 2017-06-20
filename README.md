# DeepNovo

## Synopsis

This repository contains the source code of DeepNovo. Due to large file sizes, all training and testing datasets, pre-trained models can be downloaded from the FTP server of the MassIVE database via the following link: ftp://Nancyzxll:DeepNovo2017@massive.ucsd.edu (user account:Nancyzxll, password:DeepNovo2017).

DeepNovo is implemented and tested with Python 2.7 and TensorFlow r0.10. In addition, DeepNovo also uses Numpy and Cython.

## How to use DeepNovo?
#### Note: Please download the training and testing datasets, pre-trained models via the following link: ftp://Nancyzxll:DeepNovo2017@massive.ucsd.edu (user account:Nancyzxll, password:DeepNovo2017).

    Step 0: Build cython_speedup to accelerate Python with C.
    
        python setup.py build_ext --inplace
        
    Step 1: Test a pre-trained DeepNovo using the following command.
    
        python main.py --train_dir train.deepnovo.cross.7species_50k.exclude_yeast --decode --beam_search --beam_size 10
        
        There are seven pre-trained models located in the following folders:
            train.deepnovo.cross.7species_50k.exclude_celegans
            train.deepnovo.cross.7species_50k.exclude_ecoli
            train.deepnovo.cross.7species_50k.exclude_fruitfly
            train.deepnovo.cross.7species_50k.exclude_human
            train.deepnovo.cross.7species_50k.exclude_mouse
            train.deepnovo.cross.7species_50k.exclude_pseudomonas
            train.deepnovo.cross.7species_50k.exclude_yeast
            
        The testing mgf file is defined in "data_utils.py", for example:
            decode_test_file = "data/yeast.full/peaks.db.10k.mgf"
            decode_test_file = "data/yeast.full/peaks.db.mgf"
        
        The results are written to "decode_output.tab" in the model folder.
            
    Step 2: Train a DeepNovo model using the following command.
      
        python main.py --train_dir train.example --train

        You can prepare your own training data by using annotated spectra in MGF format, preferably several hundred thousand. One MGF file should include a line like SEQ=AKNRLK of each spectrum. To build the model, you had better divide your training data into three parts with the ratios of 90%, 5% and 5%. Then rename them with the suffixes as "train.repeat", "valid.repeat", and "test.repeat". Put the path to the training file in "data_utils.py".
        
        For example, in "data_utils.py", write:
            data_format = "mgf"
            input_file_train = "data/cross.9high_80k.exclude_bacillus/cross.cat.mgf.train.repeat"
            input_file_valid = "data/cross.9high_80k.exclude_bacillus/cross.cat.mgf.valid.repeat"
            input_file_test = "data/cross.9high_80k.exclude_bacillus/cross.cat.mgf.test.repeat"
        
        The model files are then written to the training folder "train.example".
                
    Step 3: De novo sequencing.
    
        Suppose that your trained model is stored in "train.example". 
        Test your trained DeepNovo using the following command.
    
        python main.py --train_dir train.example --decode --beam_search --beam_size 10
        
        The testing mgf file is defined in "data_utils.py", for example:
            decode_test_file = "data/high.bacillus.PXD004565/peaks.db.10k.mgf"
            decode_test_file = "data/high.bacillus.PXD004565/peaks.db.mgf"
        
        The results are written to "decode_output.tab" in the model folder "train.example".
    
        Currently DeepNovo supports training and testing modes. Hence, the real peptides need to be provided in the input mgf files with tag "SEQ=". If you want to do de novo sequencing, you can provide any arbitraty sequence for tag "SEQ=" to bypass the reading procedure. In the output file, you can ignore the calculation of accuracy and simply use the predicted peptide sequence.
            
    All other options can be found in "data_utils.py".
