
using EyeStepPackage;

// . . .
{
    private void Form1_Load(object sender, EventArgs e)
    {
        EyeStep.open("RobloxPlayerBeta.exe");
    
        // yes, it disassembles!!
        var random_instruction = EyeStep.read(EyeStep.base_module + 0x1027).data;
    
        var deserializer = scanner.scan_xrefs(": bytecode").Last();
        deserializer = util.getPrologue(deserializer);
    
        MessageBox.Show("Deserializer: " + util.raslr(deserializer).ToString("X8"));
        
        
        
        // Here is a demonstration of how to use the EyeStep
        // External function calling remote.
        
        // Did I mention it can also bypass retcheck via spoofing?
        EmRemote fRemote = new EmRemote();
        fRemote.Load();
        
        var a_messagebox = imports.GetProcAddress(imports.GetModuleHandle("USER32.dll"), "MessageBoxA");
        fRemote.Add("MessageBox", a_messagebox, "int", "string", "string", "int" ); // load the function/typedef
        
        fRemote.Call("MessageBox", 0, "Test", "Test2", 0);  // call the function externally
        fRemote.Call("MessageBox", 0, "Hello", "Whats up!", 0);
        
        var a_index2adr = util.aslr(0x13D8570);
        var rL = util.debug_r32(a_index2adr + 3, EyeStep.R32_EBP, 8, 1)[0];
        MessageBox.Show("ROBLOX Lua State: " + rL.ToString("X8"));
            
        
        // Here is a demonstration of calling a lua function.
        // Use `AddProtected` to bypass any possible return checks
        // that are present in the function:
        
        fRemote.AddProtected("lua_newthread", util.aslr(0x13D8FB0), "int" );
        
        // call lua_newthread and set rL to the new lua state returned
        rL = (int)fRemote.Call("lua_newthread", rL).Item1; 
        
        MessageBox.Show("NEW Lua State Thread: " + rL.ToString("X8"));
        
    }
}
