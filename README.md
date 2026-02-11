  ---                                                                                                                  
  # 🚀 Local AI Coding Setup                                                                                           
  Ollama + OpenCode (macOS Terminal Only)                                                                              
                                                                                                                       
  Minimal, fast, no GUI, perfect for developers.                                                                       
                                                                                                                       
  Tested on:                                                                                                           
  - macOS                                                                                                              
  - Apple Silicon (M1/M2/M3)                                                                                           
  - Node/npm installed                                                                                                 
  - brew installed                                                                                                     
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 1. Install Ollama (local LLM runner)                                                                              
  --------------------------------------------------                                                                   
                                                                                                                       
  Install:                                                                                                             
                                                                                                                       
      brew install ollama                                                                                              
                                                                                                                       
  Test:                                                                                                                
                                                                                                                       
      ollama --version                                                                                                 
                                                                                                                       
  Run server (required for OpenCode):                                                                                  
                                                                                                                       
      ollama serve                                                                                                     
                                                                                                                       
  Keep this running in a terminal tab or run in background.                                                            
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 2. Download coding models                                                                                         
  --------------------------------------------------                                                                   
                                                                                                                       
  Recommended (best quality/speed balance):                                                                            
                                                                                                                       
      ollama pull qwen2.5-coder:7b                                                                                     
                                                                                                                       
  Alternatives:                                                                                                        
                                                                                                                       
      ollama pull deepseek-coder:6.7b                                                                                  
      ollama pull codellama:7b                                                                                         
                                                                                                                       
  List installed models:                                                                                               
                                                                                                                       
      ollama list                                                                                                      
                                                                                                                       
  Remove a model:                                                                                                      
                                                                                                                       
      ollama rm model-name                                                                                             
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 3. Find Ollama API endpoint                                                                                       
  --------------------------------------------------                                                                   
                                                                                                                       
  Ollama runs a local API server. To find the endpoint:                                                                
                                                                                                                       
      curl http://localhost:11434/api/tags                                                                             
                                                                                                                       
  If it returns JSON with your models, Ollama is running on port 11434.                                                
                                                                                                                       
  Default endpoints:                                                                                                   
  - Ollama native API: http://localhost:11434                                                                          
  - OpenAI-compatible API: http://localhost:11434/v1                                                                   
                                                                                                                       
  OpenCode needs the OpenAI-compatible endpoint (/v1), because it uses                                                 
  the @ai-sdk/openai-compatible package to communicate with Ollama.                                                    
                                                                                                                       
  You can also check with:                                                                                             
                                                                                                                       
      curl http://localhost:11434/v1/models                                                                            
                                                                                                                       
  This returns models in OpenAI format.                                                                                
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 4. Configure model context window (IMPORTANT)                                                                     
  --------------------------------------------------                                                                   
                                                                                                                       
  Ollama defaults to 4K context which is too small for OpenCode.                                                       
  You need at least 16K for agentic workflows.                                                                         
                                                                                                                       
  Create a model with increased context:                                                                               
                                                                                                                       
      ollama run qwen2.5-coder:7b                                                                                      
      /set parameter num_ctx 16384                                                                                     
      /save qwen2.5-coder:7b-16k                                                                                       
      /bye                                                                                                             
                                                                                                                       
  This creates a new model variant qwen2.5-coder:7b-16k with 16K context.                                              
                                                                                                                       
  For even better results (if you have RAM):                                                                           
                                                                                                                       
      ollama run qwen2.5-coder:7b                                                                                      
      /set parameter num_ctx 32768                                                                                     
      /save qwen2.5-coder:7b-32k                                                                                       
      /bye                                                                                                             
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 5. Test model in terminal                                                                                         
  --------------------------------------------------                                                                   
                                                                                                                       
  Interactive chat:                                                                                                    
                                                                                                                       
      ollama run qwen2.5-coder:7b-16k                                                                                  
                                                                                                                       
  Example prompt:                                                                                                      
                                                                                                                       
      write a wordpress ajax handler in php                                                                            
                                                                                                                       
  Exit with /bye                                                                                                       
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 6. Install OpenCode CLI                                                                                           
  --------------------------------------------------                                                                   
                                                                                                                       
  Global install:                                                                                                      
                                                                                                                       
      npm install -g opencode-ai                                                                                       
                                                                                                                       
  Test:                                                                                                                
                                                                                                                       
      opencode --help                                                                                                  
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 7. Connect OpenCode to Ollama                                                                                     
  --------------------------------------------------                                                                   
                                                                                                                       
  Option A: Automatic (easiest)                                                                                        
                                                                                                                       
  Ollama has a launch command that configures everything:                                                              
                                                                                                                       
      ollama launch opencode                                                                                           
                                                                                                                       
  This auto-configures OpenCode to use your Ollama models.                                                             
                                                                                                                       
                                                                                                                       
  Option B: Manual configuration                                                                                       
                                                                                                                       
  Create config folder:                                                                                                
                                                                                                                       
      mkdir -p ~/.config/opencode                                                                                      
                                                                                                                       
  Create config file:                                                                                                  
                                                                                                                       
      nano ~/.config/opencode/opencode.json                                                                            
                                                                                                                       
  Paste this configuration:                                                                                            
                                                                                                                       
      {                                                                                                                
        "$schema": "https://opencode.ai/config.json",                                                                  
        "provider": {                                                                                                  
          "ollama": {                                                                                                  
            "npm": "@ai-sdk/openai-compatible",                                                                        
            "name": "Ollama",                                                                                          
            "options": {                                                                                               
              "baseURL": "http://localhost:11434/v1"                                                                   
            },                                                                                                         
            "models": {                                                                                                
              "qwen2.5-coder:7b-16k": {                                                                                
                "tools": true                                                                                          
              }                                                                                                        
            }                                                                                                          
          }                                                                                                            
        },                                                                                                             
        "model": "ollama/qwen2.5-coder:7b-16k"                                                                         
      }                                                                                                                
                                                                                                                       
  Save with Ctrl+O, Enter, Ctrl+X.                                                                                     
                                                                                                                       
  Key points:                                                                                                          
  - Config path: ~/.config/opencode/opencode.json (NOT ~/.opencode/)                                                   
  - Ollama requires @ai-sdk/openai-compatible npm package                                                              
  - baseURL must be http://localhost:11434/v1 (the /v1 is required!)                                                   
  - "tools": true enables function calling for agentic features                                                        
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 8. Use OpenCode                                                                                                   
  --------------------------------------------------                                                                   
                                                                                                                       
  Start OpenCode in any project directory:                                                                             
                                                                                                                       
      cd /path/to/your/project                                                                                         
      opencode .                                                                                                       
                                                                                                                       
  This opens the interactive TUI (terminal UI).                                                                        
                                                                                                                       
  Commands inside OpenCode:                                                                                            
  - Type your question/request and press Enter                                                                         
  - Use Ctrl+A to switch models (if multiple configured)                                                               
  - Use /help for available commands                                                                                   
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 9. Multiple models setup                                                                                          
  --------------------------------------------------                                                                   
                                                                                                                       
  Add multiple models to your config:                                                                                  
                                                                                                                       
      {                                                                                                                
        "$schema": "https://opencode.ai/config.json",                                                                  
        "provider": {                                                                                                  
          "ollama": {                                                                                                  
            "npm": "@ai-sdk/openai-compatible",                                                                        
            "name": "Ollama",                                                                                          
            "options": {                                                                                               
              "baseURL": "http://localhost:11434/v1"                                                                   
            },                                                                                                         
            "models": {                                                                                                
              "qwen2.5-coder:7b-16k": {                                                                                
                "tools": true                                                                                          
              },                                                                                                       
              "deepseek-coder:6.7b": {                                                                                 
                "tools": true                                                                                          
              },                                                                                                       
              "codellama:7b": {                                                                                        
                "tools": true                                                                                          
              }                                                                                                        
            }                                                                                                          
          }                                                                                                            
        },                                                                                                             
        "model": "ollama/qwen2.5-coder:7b-16k",                                                                        
        "small_model": "ollama/codellama:7b"                                                                           
      }                                                                                                                
                                                                                                                       
  The small_model is used for lightweight tasks like generating titles.                                                
                                                                                                                       
  Switch models inside OpenCode with Ctrl+A.                                                                           
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 10. Suggested models by task                                                                                      
  --------------------------------------------------                                                                   
                                                                                                                       
  General coding:     qwen2.5-coder:7b      Best balance                                                               
  WordPress/PHP:      deepseek-coder:6.7b   Good for PHP                                                               
  Fast/low RAM:       codellama:7b          Lighter model                                                              
  Heavy quality:      qwen2.5-coder:32b     Needs 32GB+ RAM                                                            
                                                                                                                       
  Remember to create 16K+ context variants for each model you use.                                                     
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 11. Troubleshooting                                                                                               
  --------------------------------------------------                                                                   
                                                                                                                       
  OpenCode doesn't find models:                                                                                        
  - Make sure ollama serve is running                                                                                  
  - Check baseURL is http://localhost:11434/v1                                                                         
  - Verify model name matches exactly: ollama list                                                                     
                                                                                                                       
  Tool calls not working:                                                                                              
  - Context window too small (increase num_ctx to 16K+)                                                                
  - Model doesn't support tools well (try qwen2.5-coder)                                                               
                                                                                                                       
  Config errors:                                                                                                       
  - Check JSON syntax (no trailing commas)                                                                             
  - Use the schema                                                                                                     
                                                                                                                       
  Test Ollama is running:                                                                                              
                                                                                                                       
      curl http://localhost:11434/api/tags                                                                             
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## 12. Useful commands                                                                                               
  --------------------------------------------------                                                                   
                                                                                                                       
  Check Ollama status:                                                                                                 
                                                                                                                       
      curl http://localhost:11434/api/tags                                                                             
                                                                                                                       
  Check OpenAI-compatible endpoint:                                                                                    
                                                                                                                       
      curl http://localhost:11434/v1/models                                                                            
                                                                                                                       
  List models:                                                                                                         
                                                                                                                       
      ollama list                                                                                                      
                                                                                                                       
  Pull new model:                                                                                                      
                                                                                                                       
      ollama pull model-name                                                                                           
                                                                                                                       
  Start OpenCode:                                                                                                      
                                                                                                                       
      opencode .                                                                                                       
                                                                                                                       
                                                                                                                       
  --------------------------------------------------                                                                   
  ## Done                                                                                                              
  --------------------------------------------------                                                                   
                                                                                                                       
  You now have:                                                                                                        
  - Fully local AI                                                                                                     
  - No cloud dependencies                                                                                              
  - No GUI required                                                                                                    
  - Fast terminal workflow                                                                                             
  - Multi-model support                                                                                                
  - Agentic coding capabilities (tool use)                                                                             
                                                                                                                       
  Resources:                                                                                                           
  - OpenCode Docs: https://opencode.ai/docs/                                                                           
  - Ollama Docs: https://docs.ollama.com/                                                                              
  - Ollama + OpenCode Guide: https://github.com/p-lemonish/ollama-x-opencode                                           
                                                                                                                       
  ---    
