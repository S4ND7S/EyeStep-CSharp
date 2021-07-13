// Simply include all 3 EyeStep files to compile
// this project.
// EyeStep.cs, EyeStepUtility.cs, EyeStepRemote.cs


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
        
        
    }
}
