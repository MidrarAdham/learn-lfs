```
graph TD
    Start([Start: Main Execution]) --> Prep[Prepare Transformer Data]
    
    subgraph Data_Processing [Data Preparation Phase]
        Prep --> Load[Load CSV & Filter Rows 1440-2880]
        Load --> Cleanup[Cleanup Timestamps & Power Values]
        Cleanup --> Resample[Resample to 10-min Intervals]
        Resample --> Binary[Create Binary States: ON=1, OFF=0]
    end

    Binary --> BayesInit[Initialize Bayesian Parameters]

    subgraph Bayesian_Logic [Bayesian Implementation Loop]
        BayesInit --> Params[Set alpha=1, beta=1, discount=0.3]
        Params --> Hist[Initialize History Dictionary]
        Hist --> Loop{Iterate 144 Chunks}
        
        Loop --> Slice[Slice 10-point Window]
        Slice --> Count[Count Heads/ON and Tails/OFF]
        Count --> Posterior[Update Posterior Alpha & Beta]
        Posterior --> Stats[Calculate Mean & Confidence Intervals]
        Stats --> Record[Record Stats in History]
        Record --> Loop
    end

    Loop -- All Chunks Processed --> Matrix[Create Results Matrices]
    
    subgraph Evaluation [Performance Analysis]
        Matrix --> Metrics[Quantify Error Metrics]
        Metrics --> Error[Calculate MAE, RMSE, R-squared, NRMSE]
    end

    Error --> End([End])

    %% Citations and specific logic links
    %% [1] initialize_history
    %% [2] cleanup_results_files, create_binary_states
    %% [3] bayesian_implementation core loop & posterior formulas
    %% [4] prepare_transformer_data, create_matrices
    %% [5] quantifying_error_metrics
```
