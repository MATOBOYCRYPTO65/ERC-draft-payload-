# ERC-draft-payload-
Secure Transaction Verification and Authorization (STVA)
struct VerificationPayload {
    bytes32 txHash;                 // keccak256 of the canonical transaction or UserOperation
    address target;                 // primary contract being called (or account for AA)
    bytes   callData;               // exact calldata
    uint256 value;
    uint256 chainId;
    uint256 nonce;
    string  semanticIntent;         // human-readable summary (MUST be generated deterministically or attested)
    bytes32 intentHash;             // keccak256 of structured intent fields
    SimulationResult expectedResult; // optional but recommended
    Capability[] capabilities;      // scoped capabilities being exercised
    bytes   metadata;               // additional structured data (JSON or ABI-encoded)
}
interface ISTVASimulator {
    struct SimulationResult {
        bool success;
        bytes returnData;
        StateChange[] stateChanges;   // balance, storage, code, logs, etc.
        string[] warnings;            // e.g., "delegatecall to unknown implementation"
        bytes32 resultHash;           // canonical hash of the entire result
    }

    function simulate(VerificationPayload calldata payload) external view returns (SimulationResult memory);
}
struct Capability {
    address granter;
    address grantee;          // account or module
    bytes32 actionId;         // e.g., keccak256("UPGRADE_IMPLEMENTATION")
    address target;
    uint256 maxValue;
    uint256 expiration;
    bytes32 salt;
    bytes   constraints;      // optional further limits
}
