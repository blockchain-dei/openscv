<h1 align="center">OpenSCV: An Open Hierarchical Taxonomy for Smart Contract Vulnerabilities</h1>


OpenSCV is a taxonomy for smart contract vulnerabilities, proposed in: Vidal FR, Ivaki N, Laranjeiro N (2023d) Openscv: "An open hierarchical taxonomy for smart contract vulnerabilities". URL https://doi.org/10.48550/arXiv.2303.14523.

<p>The taxonomy is visually available at https://openscv.dei.uc.pt and a summary is available in the next paragraphs. This repository also contains a dataset holding vulnerable and corrected versions of contracts, for each of the described vulnerabilities.</p>

<h2>1. Unsafe External Calls</h2>
<p>This category represents a set of vulnerabilities in which there is an interaction between at least two contracts. It also refers to vulnerabilities related to contracts making non-blockchain external calls, such as calling external web services, calling external libraries, executing external commands, or accessing external files.</p>


<h3>1.1 Reentrancy</h3>
<p>The first subcategory is reentrancy, in which two contracts are involved: the vulnerable contract and the malicious contract. Overall, this type of vulnerability occurs when the malicious contract, after initiating a call, is allowed to make new calls to the vulnerable contract before the initial call has been completed. Thus, unexpected state changes may occur, such as depletion of credit. We identified two types of reentrancy vulnerabilities: one associated with loss of credit and the other associated with unexpected state changes. This is in line with several vulnerability detection tools, such as Securify (Tsankov, 2018), Slither (Slither’s Github, 2019), or (Momeni et al., 2019) (Feist et al., 2019), which also distinguish these two cases (although using different names).</p>


<i>1.1.1 Unsafe Credit Transfer</i>
<p>Known due to the DAO attack event \cite{Siegel2016}, this vulnerability allows attackers to maliciously change balance via credit transfer calls that are allowed to take place before a previous call has been completed. Let us consider the case where a smart contract maintains the balance of several addresses, allowing the retrieval of funds. A malicious contract may initiate a withdrawal operation which would lead the vulnerable contract to send funds to the malicious one before updating the balance of the malicious contract. Funds would be accepted on the malicious contract side, and a new withdrawal could be initiated (before the balance had been updated on the vulnerable contract side). As a consequence, the malicious contract could withdraw funds multiple times, with the total sum exceeding its own funds.</p>

<p>Using the Orthogonal Defect Classification (ODC) as a reference, this defect can be classified as being of type Algorithm as the nature of the defect sits in the logic created by the programmer. The ODC qualifier is defined as wrong as the error is related to incorrect logic (i.e., not missing or extraneous logic), related to the order of the instructions in the code.</p>

<p>Regarding the relationship to CWE, we classify this vulnerability (and actually the whole reentrancy group) as CWE-841, which describes a situation where "the software supports a session in which more than one behavior must be performed by an actor, but it does not properly ensure that the actor performs the behaviors in the required sequence". This vulnerability is also known in the literature as "simple reentrancy" (Li et al., 2022a), "modified reentrancy" (Li et al., 2022a), "reentrancy-eth" (Li et al., 2022d), "cross-function re-entrancy" (Mavridou et al., 2019), "DAO" (Tsankov et al., 2018), "buggy-locked reentrancy" (Li et al., 2022a), "reentrancy vulnerabilities"
(Chinen et al., 2020), "reentrancy" (Gupta et al., 2022; Sun et al., 2023; Chen et al., 2020; Hwang et al., 2022; Yu et al., 2021; Eshghie et al., 2021; Ashizawa et al., 2021; Zeng et al., 2022; Hu et al., 2023; Xue et al., 2022; Zhang et al., 2022a; Fu et al., 2019; Ma et al., 2022; Wu et al., 2021; Rodler et al., 2019; Bose et al.,2022; Choi et al., 2021; Liao et al., 2022; Zhou et al., 2022b; Mi et al.,2021; Ye et al., 2022; Liu et al., 2021; Zhuang et al., 2020; Ashouri, 2020; Ashraf et al., 2020; Feist
et al., 2019; Jiang et al., 2018; Kalra et al., 2018; Luu et al., 2016; Wang Zexu and Wen et al., 2021; Mavridou et al., 2019; Nguyen et al., 2020; Stephens et al., 2021; Song et al., 2019; Tikhomirov et al., 2018; Wang et al., 2021, 2019; Xue et al., 2020; Akca et al., 2019; Torres et al., 2021; Zhang et al., 2022c), or "SWC-107 reentrancy" (SmartContractSecurity, 2020).</p>

<i>1.1.2 Unsafe System State Changes</i>

<p>This vulnerability is similar in nature to v1.1.1, with the main difference being the fact that there is no credit involved and, thus, no impact on users’ funds. Due to the way the contract is coded, a call that reaches the vulnerable contract before a previous one has ended may allow an attacker to place the program in an unexpected state, leading to various effects, depending on the type of contract involved, including performance or availability issues. This vulnerability is also known in the literature as "call to the unknown" (Atzei et al., 2017) or "unexpected function "invocation(Chen et al., 2020).</p>

<h3>1.2 Malicious Fallback Function</h3>
<p>
Fallback functions are functions that are executed when a program receives a call to a function whose signature does not exist, i.e., either the name does not exist or the parameters do not match the parameters of any of the existing functions. For instance, an attacker could deploy a smart contract with a malicious fallback function, which could be used to drain funds or alter the system’s state. By mistake, a user could invoke it and reach a state that was not expecting to reach (Chen et al., 2020). This vulnerability is also known in the literature as "Call to the unknown" (Atzei et al., 2017; Argañaraz et al., 2020; Chapman et al., 2019) or "Unexpected function invocation" (Chen et al., 2020).</p>


<h3>1.3 Improper Check of External Call Result</h3>

<p> This category groups vulnerabilities that verify the execution of external contracts in an improper manner (i.e., verification is wrong or even missing), which affects the subsequent logic of the calling contract. The result of invoking a certain external operation should be verified, first of all, because it may simply fail, but especially because the called operation may be malicious (or may just have been poorly coded, resulting in an unexpected result); thus, the direct use of the result may lead to unexpected behavior.</p>

<i>1.3.1 Improper Check of External Call Return Value</i>

<p>This defect consists of an incorrect (or missing) verification of the returned value from the external execution of a contract. When a smart contract invokes another one, the returned value should be verified because the called operation may return an unexpected value (i.e., either because the callee is malicious or may just have been poorly coded, resulting in an unexpected result) (Chen et al., 2020). This vulnerability is also known in the literature as "call-stack depth attack" (Song et al., 2019;
Wang et al., 2021), "call depth" (Sun et al., 2023; Zhang et al., 2022a), "no check after contract invocation" (Chen et al., 2020), "unchecked call return value" (Zheng et al., 2021), "unchecked external call" (Tikhomirov et al., 2018), "unused Return" (Tsankov et al., 2018), "unchecked return values" (Fu et al., 2019), or "SWC-104 Unchecked Call Return Value" (SmartContractSecurity, 2020).</p>

<i>1.3.2 Improper Exception Handling of External Calls</i>

<p>In the case of this defect, the problem resides in the incorrect (or missing) handling of exceptional behavior thrown by a call (i.e., instead of residing in the handling of values, as in the case of vulnerability v1.3.1 ). The improper verification of exceptions thrown by the callee may lead to unexpected behavior in the caller contract. There are various reasons why the callee may exhibit exceptional behavior. For instance, the callee could be under malicious control, the execution of the
transaction could activate a fault in the callee contract, the transaction could be terminated due to reaching the gas limit, or the callee contract may have been terminated (e.g., after a software fault has been detected in the contract). This vulnerability is also known in the literature as "denial of service (Ashouri, 2020)", "doS by external contract" (Tikhomirov et al., 2018), "dos attack" (Liao et al., 2022), "external contract referencing" (Mavridou et al., 2019) or "SWC-113 DoS with Failed
Call" (SmartContractSecurity, 2020).</p>


<i>1.3.3 Improper Check of Low-Level Call Return Value</i>
<p>Languages like Solidity offer the possibility of using low-level calls that operate over raw addresses.
Such calls do not verify that the code exists or the success of the calls. Thus, its use may lead to unexpected behavior (Xi and Pattabiraman, 2023). As a result, using such calls can be risky and should be avoided in most cases. This vulnerability is also known in the literature as "check effects" (Zhang et al., 2022a), "inline assembly" (Zhang et al., 2022a), "low level calls (Tsankov et al., 2018; Zhang et al., 2022a), "unchecked call" (Hu et al., 2023), "unchecked low-level calls" (Hwang et al., 2022), or "low-level-calls" (Li et al., 2022d). </p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20 at the time of writing. If encountered, the compiler provides the following informative warning message: "Warning: Return value of low-level calls not used"</p>


<h3>1.4 Improper Locking During External Calls</h3>

<p>A vulnerable contract uses a lock mechanism in an erroneous manner, which may cause deadlocks. This may result, for instance, in the impossibility of executing transfers and eventually in Denial of Service (Mavridou et al., 2019). This vulnerability is also known in the literature as "Deadlock-freedom" (Mavridou et al., 2019) or "SWC-132 Unexpected Ether balance"(SmartContractSecurity,2020).</p>

<h3>1.5 Interoperability Issues with Other Contracts</h3>

<p> This issue relates to interoperability issues between contracts built in different language versions. Newer contracts may execute or inherit discontinued functionality present in older contracts (Khan et al., 2021). For instance, Solidity has introduced the operation code STATICCALL to allow a contract
to call another contract (or itself) without modifying the state. Starting from V0.5.0, pure and view functions must now be called using the code STATICCALL instead of the usual CALL code. Consequently, when defining an interface for older contracts, the programmer should only use view instead of constant in the case s/he is absolutely sure that the function will work with STATICCALL (Solidity, 2023). This vulnerability is also known in the literature as "assembly Usage" (Tsankov et al., 2018)</p>

<h3>1.6 Delegatecall to Untrusted Callee</h3>

<p>Calling untrusted contracts using the delegate feature is generally highly problematic because it opens the possibility for the called contract to change sensitive variables (e.g., msg.data or sender) of the source contract (Jiang et al., 2018). This type of issue has been most notably known as the Parity hack, which allowed attackers to reset the ownership and usage arguments of existing user wallets (Krupp and Rossow, 2018). This vulnerability is also known as "Unrestricted delegate
call" (Tsankov et al., 2018), "Dangerous delegate call" (Jiang et al., 2018; Ashraf et al., 2020), "Unchecked delegate call function" (Nguyen et al., 2020), "Code injection" (Krupp and Rossow, 2018), "Control-flow Hijack" (Choi et al., 2021), "Delegated call" (Andesta et al., 2020; Li et al., 2023; Hu et al., 2023), "Cross Program Invocation" (Cui et al., 2022), "Tainted delegatecall" (Brent et al., 2020) or "SWC-112 Delegatecall to Untrusted Callee" (SmartContractSecurity, 2020).</p>

<h3>1.7 Unsafe Non-Blockchain External Call</h3>

<p>This category of vulnerabilities is related to contracts i) making non-blockchain external calls to untrusted third-parties resources like web services and libraries, ii) executing external commands, or iii) accessing external files. These calls or accesses to non-blockchain external resources will cause security issues if different results are returned to each peer node, which consequently will result in inconsistent endorsements</p>

<i>1.7.1 Unsafe External Web Service Call</i>

Sometimes, contracts may opt to streamline the development process and reduce effort by reusing external web services through API calls. Nevertheless, this approach
can potentially lead to security issues if the returned values vary across different peer nodes. This vulnerability is also known in the literature as "Web service" (Li et al., 2022b).

<i>1.7.2 Unsafe External Library Call</i>

Similar to the previously mentioned vulnerability, contracts might incorporate external libraries without a comprehensive understanding of their internal workings. This vulnerability is also known in the literature as "External Library Calling" (Li et al., 2022b).

<i>1.7.3 Unsafe External Command Execution</i>

External command execution is another possibility within smart contracts. Nevertheless, this action cannot guarantee consistent results across all peer nodes. This vulnerability is also known in the literature as "System Command Execution" (Li et al., 2022b).

<i>1.7.4 Unsafe External File Access</i>

Like external command execution, smart contracts also enable access to external files, but it cannot be assured that the nodes will receive identical results. This vulnerability is also known in the literature as "External File Accessing" (Li et al., 2022b).

<h3>1.8 Cross Channel Invocation</h3>

Certain blockchain platforms like Hyperledger Fabric permit contracts to call each other. However, when two contracts interact through different channels, inconsistencies can arise in message reception. This vulnerability is known in the literature with the same name (Li et al., 2022b).


<h2>2. Mishandled Events</h2>
<p>This category includes a set of vulnerabilities in which exceptional events are mishandled. In Solidity, specific functions can be used to verify if certain conditions exist and throw exceptions in case the conditions are not met, namely require and assert. There are, however, fundamental differences. When the "require" function returns false, all executed changes are reverted, and all remaining gas fees are refunded. When the assert function returns false, it reverts all changes but consumes all
remaining gas. However, such differences have become a frequent source of problems (Hajdu and Jovanović, 2020).</p>

<h3>2.1 Improper Exceptional Events Handling</h3>


<p>This first group of vulnerabilities is directly related to exceptional events, which, when mishandled, are often linked to the loss of atomicity in operations and other effects, such as excessive gas consumption or unauthorized access.</p>

<i>2.1.1 Improper Use of Exception Handling Functions</i>

<p>Diverse run-time errors (e.g., out-of-gas error, data type overflow error, division by zero error, array-out-of-index error, etc.) may happen after a compiled smart contract is deployed. However, Solidity has many functions for error handling (e.g., throw, assert, require, revert), but their correct use relies on the experience and expertise of the developer. This defect occurs when the developer misuses the handling exception functions, which can lead the program to unexpected behavior. This
vulnerability is also known in the literature as "exception disorder" (Jiang et al., 2018), "exception state" (Zhou et al., 2022b), "mishandled exceptions (Choi et al.,2021; Fu et al., 2019; Luu et al., 2016; Mavridou et al., 2019; Nguyen et al., 2020), "unexpected revert" (Ye et al., 2022), "unhandled errors" (Li et al., 2022b), "unhandled exception" (Ashouri, 2020; Torres et al., 2021), or "unhandled
exception" (Tsankov et al., 2018).</p>

<i>2.1.2 Improper Exception Handling in a Loop</i>

<p>This vulnerability occurs when a transaction is excessively large (i.e., it executes too many statements) and may lead to excessive costs. For instance, when one of the statements in a transaction fails (e.g., due to a software bug), the transaction will not be packaged into a block, and the consumed gas will not be returned to the user (and actually the concluded operations are reverted and must be executed again). Thus, such kinds of transactions should be decomposed into smaller parts so that the likelihood of success increases and the negative effects associated with the failure cases diminish. This vulnerability is also known in the literature as "call in loop"(Tsankov et al., 2018), "costly loop" (Shakya et al., 2022; Tikhomirov et al., 2018), "expensive operations in a loop" (Chen et al., 2021), "fusible loops" (Chen et al., 2021), "repeated computation in a loop" (Chen et al.,2021), "revert DOS" (Stephens et al., 2021), "unilateral comparison in a loop" (Chen et al., 2021), "unbounded mass operation" (Grech et al., 2020), "costly-operations-loop" (Li et al., 2022d), "gas limit DoS on a contract via unbounded operations" (Nassirzadeh Behkish and Sun et al., 2023), or "SWC-128: DoS With Block Gas Limit" (SmartContractSecurity, 2020).</p>

<i>2.1.3 Incorrect Revert Implementation in a Loop</i>

<p>In the case of this vulnerability, the developer incorrectly specifies how the revert operation should be handled (in the context of a loop or a transaction composed of multiple operations), which ends up in a partial revert of the whole set of operations that should be reverted. This vulnerability is also known in the literature as "non-isolated calls (wallet griefing)" (Grech et al., 2020), "push DOS"(Stephens et al., 2021), or "SWC-126 Insufficient Gas Griefing" (SmartContractSecurity, 2020).</p>

<h3>2.2 Improper Token Exception Handling</h3>

<p>The ERC-20 standard (Vogelsteller and Buterin, 2015) provides functionalities to exchange tokens. Besides describing the functionalities, the standard specifies good practices for developers to implement its features. Regarding the transfer function, exceptional events can become problematic if handled improperly</p>

<i>2.2.1 Missing Thrown Exception</i>

<p>Regarding the transfer function (i.e., functionality to transfer tokens from one account to another), the ERC-20 standard recommends to the developer throw an exception when a condition of the caller’s account balance does not have enough tokens to spend. This allows the caller to understand the reason for which the transfer is not completed and take appropriate action. This vulnerability is also known in the literature as "ERC-20 transfer" (Ashizawa et al., 2021), "missing the transfer event" (Chen et al., 2020), or "non-standard implementation of tokens" (Ji et al., 2020).</p>

<i>2.2.2 Extraneous Exception Handling</i>

<p>This type of defect refers to the implementation of extra actions compared to what is recommended in a certain specification. The specification does not recommend actions like using guard functions (e.g., require or assert) in addition to throwing an exception when there is no balance in the caller. The extra actions might be arbitrary and incompatible with the purpose of a transfer functionality (e.g., returning true or false to report the success of the execution). This vulnerability is also known in the literature as "flawed back-end Verification of CEXes" (Ji et al., 2020), "infinite loop" (Liu et al., 2021; Zhuang et al., 2020), or "token API violation"(Tikhomirov et al., 2018).</p>

<h2>3. Gas Depletion</h2>

<p>This category groups defects that, in different ways, lead to gas depletion of the account used for
the smart contract execution.</p>

<h3>3.1 Improper Gas Requirements Checking</h3>

<p>This defect represents missing or wrong checking of the prerequisites (i.e., in terms of gas) for executing a certain operation, causing unnecessary processing and use of memory resources. For cost management reasons, languages offer programmers several ways to deal with the cost of executing a certain operation in a contract. For instance, for transferring credits, Solidity provides the functions transfer() and send(), which have a limit of 2300 gas units for each execution. An alternative is to build a custom transfer function, where the gas limit is defined by a variable (e.g., address.call.value(ethAmount).gas(gasAmount)()). Despite having several ways of managing the program costs, it is challenging for programmers to predict which part of the code may fail. If an out-of-gas exception is triggered, the result may be unexpected behavior. This vulnerability is also known in the literature as "extra gas consumption" (Shakya et al., 2022), "gas consumption" (Ashizawa et al., 2021), "gas Dos" (Stephens et al., 2021), "gas less send(Ashraf et al., 2020; Nguyen et al., 2020; Jiang et al., 2018; Wang et al., 2019), "opaque predicate" (Chen et al., 2021), "out of gas" (Akca et al., 2019), or "SWC-126 Insufficient Gas Griefing" (SmartContractSecurity, 2020) </p>

<h3>3.2 Call with Hardcoded Gas Amount</h3>

<p>This defect refers to the impossibility of adjusting the amount of gas a certain program uses after being deployed. This issue is related to the observation that certain transfer credit in real contracts was being deployed using a fixed amount of gas (i.e., 2300 gas). If the gas cost of EVM instructions changes during, for instance, a hard fork, previously deployed smart contracts will easily break. This vulnerability is also known in the literature as "SWC-134 Message call with hardcoded gas amount" (SmartContractSecurity, 2020).</p>

<h2>4. Erroneous Credit Transfer</h2>

<p>This category groups defects which are generally related to improper credit transfer operations</p>

<h3>4.1 Improper Check on Transfer Credit</h3>
<p>This defect refers to the absence of verification (or wrong verification) after a transfer event, which can lead to an erroneous vision of the correct balance of the account. Indeed, the balance of the account may not reflect the currency transferred in an exact manner, leading to potential errors and opening the door to security issues. This vulnerability is also known in the literature as "forged transfer notification" (Li et al., 2022c), "unchecked send" (Kalra et al., 2018; Akca et al., 2019;
Stephens et al., 2021), or "including Fake EOS transfer" (Li et al., 2022c).</p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20 at the time of writing. If encountered, the compiler provides the following informative warning message: "Warning: Failure condition of send ignored. Consider using transfer instead"</p>

<h3>4.2 Unprotected Transfer Value</h3>
<p>The transfer function uses a numeric variable for transfers and may be vulnerable if it does not protect or specify limits for the values. When attribute address.balance is used for identifying the amount to be transferred, it will result in transferring the total balance at once, which is a high-risk operation for the cases where the amount is high (Zhang et al., 2020b). This vulnerability is also known in the literature as "arbitrarily transfer" (Ma et al., 2023), "ETH transfer inside the loop" (Shakya et al., 2022), "ether leak" (Choi et al., 2021), "manipulated balance" (Hu et al.,2023), "multiple send" (Choi et al., 2021), "transfer forwards all gas" (Tikhomirov et al., 2018), "unchecked transfer value" (Zhang et al., 2020b), "unrestricted ether flow" (Tsankov et al., 2018), or "SWC-105 Unprotected Ether Withdrawal" (SmartContractSecurity, 2020)</p>


<h3>4.3 Wrong use of Transfer Credit Function</h3>
<p>Depending on the programming language, there are different ways to carry out credit transfer operations. In Solidity, transfer and send will both allow executing a credit transfer. However, in the case of a problem, transfer will abort the process with an exception, whereas send function will return false, and transaction execution is continued. An attacker may manipulate the send function and be able to continue executing a credit transfer operation without proper authorization. This vulnerability is also known in the literature as "failed send" (Kalra et al., 2018), "send instead of transfer" (Shakya et al., 2022; Tikhomirov et al., 2018)</p>

<h3>4.4 Missing Token Issuer Verification</h3>
<p>This vulnerability is related to EOSIO blockchain, in which the ‘transfer‘ function allows attackers to win the cryptocurrency without paying a ticket fee. This vulnerability is also known in the literature as "fake EOS" (Chen et al., 2022)</p>

<h3>4.5 Missing Token Verification of Exchange</h3>
<p>This vulnerability arises when an attacker can perform a fake deposit due to inadequate verification in the exchange implementation, specifically when unsafe usage of transfer or transferFrom functions is present. A potential solution for this issue involves adopting the safeTransferFrom function, which incorporates security checks before invoking the transferFrom, thereby mitigating the risk. This vulnerability is also known in the literature as "flawed token Verification of DEXes" (Ji et al., 2020)</p>

<h3>4.6 Fake Notification</h3>
<p>This vulnerability is related to the EOSIO blockchain, specifically in EOS notifications. The problem occurs when the attackers forward the notification from eosio.token to the victim and forge an EOS notification. This vulnerability is also known in the literature as "fake notification" (Chen et al., 2022) or "fake receipt" (He et al., 2021)</p>
  
<h2>5. Bad Programming Practices and Language Weaknesses</h2>

<p>This category represents issues that are mostly related to bad programming practices (i.e., error-prone or insecure coding practices) and language weaknesses, which are mostly related to insufficient protection mechanisms offered by the language, allowing the developers to make mistakes that could
be avoided, e.g., by language constructs.</p>

<h3>5.1 Bad Randomness</h3>

<p>This vulnerability is related to using the variables that control the blocks in a blockchain to generate randomness, which is not secure. Such variables may be manipulated by miners so that the randomness is subverted, compromising the security of the blockchain, with its information becoming vulnerable to attacks. In fact, generating a strong enough source of randomness can be very challenging. The use of variables like block.timestamp, blockhash, block.difficulty, and other fields is problematic as these can be manipulated by miners. For example, a miner could select a specific timestamp within a delimited range, or use powerful hardware to mine several blocks quickly, choose the block that would provide an interesting hash, and drop the remaining. This vulnerability is also known in the literature as "bad randomness" (Hu et al., 2023; Crincoli et al., 2022; Ashouri, 2020), "bump Seeds(Cui et al., 2022), "generating randomness" (Gupta et al., 2022), "random
number generation" (Li et al., 2022b), "use predictable variable" (Zhou et al., 2022b), "predicable variable dependency" (Fu et al., 2019), or "SWC-120 Weak Sources of Randomness from Chain Attributes" (SmartContractSecurity, 2020).</p>

<h3>5.2 Improper Initialization</h3>

<p>The smart contract has resources that are either not initialized or initialized incorrectly, leading to unexpected behavior.</p>

<i>5.2.1 Missing Constructor</i>

<p>A smart contract constructor is a function that is executed exactly once during the lifetime of a contract. It executes at deployment time, initializes state variables, performs a few necessary tasks that the specific contract requires, and sets the contract owner. If there is no constructor, the developer will have to implement such tasks manually, which is prone to security issues (e.g., variables may be set with incorrect values or forgotten, which may result in security problems). This vulnerability is also known in the literature as "SWC-118 Incorrect Constructor Name" (SmartContractSecurity, 2020).</p>

<i>5.2.2 Wrong Constructor Name</i>
<p>This vulnerability is related to the contracts that were published without a constructor because the programmer created a function, imagining it would behave like a constructor. Usually, the construction function has sensitive code (e.g., assignment of the owner of the contract), and by declaring a wrong function name, any user can call the function, thus, causing serious security risks. This vulnerability is also known in the literature as "erroneous constructor name" (Hu et al., 2023), "violated access control checks (VACC)" (Ghaleb et al., 2023), or "SWC-118 Incorrect Constructor Name" (SmartContractSecurity, 2020).</p>

<p> This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative error message: "Error: Functions are not allowed to have the same name as the contract. If you intend this to be a constructor, use constructor(...) ... to define it. Error: Expected identifier but got ()"
</p>

<i>5.2.3 This defect refers to the lack of initialization of variables that are used throughout the contract. Obviously, the effects can largely vary, depending on the variable itself and on the context in which is being used. This vulnerability is also known in the literature as "golang grammar error" (Li et al., 2022b), "uninitialized variables" (Feist et al., 2019), "uninitialized-local" (Tsankov et al., 2018), or"uninitialized state variable" (Tsankov et al., 2018).</p>

<i>5.2.4 Uninitialized Storage Variables</i>
<p>In Solidity, state variables are assigned to memory or storage. When a state variable is declared, it is assigned to a certain storage slot. If that variable is not initialized, it will be stored in slot 0 (the first one) of the contract’s storage. Thus, it may conflict with the next variable that is declared in the same slot, causing an address conflict. This latter variable will overwrite the first, leading to unexpected behavior. This is the reason why it is important to initialize all state variables in a smart contract so that they are set into the correct storage slots (and possible conflicts are avoided)(Antonopoulos and Wood, 2018). This vulnerability is also known in the literature as "uninitialized storage pointers" (Antonopoulos and Wood, 2018), "uninitialized Storage" (Tsankov et al., 2018; Hu et al., 2023), or "SWC-109 Uninitialized Storage Pointer" (SmartContractSecurity, 2020). </p>
  
<p> This issue has been addressed in the latest Solidity compiler, version 0.8.20 at the time of writing. If encountered, the compiler provides the following informative error message: "Error: This variable is of storage pointer type and can be accessed without prior assignment, which would lead to undefined behavior.".</p>


<i>5.2.5 Extraneous Field Declaration</i>
<p>This vulnerability occurs when the programmer leaves field declarations in the contract structure, thereby enabling direct access to these fields (i.e., as they are defined in the structure). Given the possibility of the node environment falling out of sync, the contract’s field values may diverge and become inconsistent among peer nodes. This vulnerability is also known in the literature as "field declarations" (Li et al., 2022b).</p>


<i>5.2.6 Hardcoded Address</i>
<p>This vulnerability occurs when the programmer leaves field declarations in the contract structure, thereby enabling direct access to these fields (i.e., as they are defined in the structure). Given the possibility of the node environment falling out of sync, the contract’s field values may diverge and become inconsistent among peer nodes. This vulnerability is also known in the literature as "field declarations" (Li et al., 2022b).</p>

<h3>5.3 Isolation Phenomena</h3>
<p>This category gathers vulnerabilities that occur due to blockchain synchronization systems (i.e., enforced by consensus mechanisms), which can lead a program to produce different results at different times for the same query</p>

<i>5.3.1 Phantom Reads</i>
<p>The Hyperledger Fabric provides mechanisms for reading the ledger (i.e., getPrivateDataQueryResult), similar to querying conventional databases, but with the difference that the programmer cannot decide about the isolation level of the ledger. Thus, a contract can read out-of-sync node information from the ledger (i.e., during the validation phase) computing and/or processing based on outdated information. This vulnerability is also known in the literature as "range query risks"
(Li et al., 2022b)</p>

<i>5.3.2 Dirty Reads</i>
<p>
 In the context of HF, a query may return a key value before its update within the same transaction. As a consequence, this behavior can lead to unexpected results, as the returned value might not reflect the most recent update. This vulnerability is also known in the literature as "read-write conflict" (Li et al., 2022b)
</p>

<h3>5.4 Error in Function Call</h3>
<p>
In a blockchain context, each function in a smart contract is identified by its name, input parameters, and output parameters. Thus, these items compose the function signature, which is used by the contracts to verify that the right function is being called. This category groups defects in which a developer uses a function in the wrong manner: either a wrong signature is used, wrong arguments are used, or a wrong function is called. </p>

<i>5.4.1 Wrong Function Call</i>
<p>
The issue occurs when a contract executes a certain function at a wrong address, i.e., at the address used by another function, which, however, has the same signature as the intended function. This vulnerability is also known in the literature as "type casts" (Atzei et al., 2017).</p>

<i>5.4.2 Wrong Selection of Guard Function</i>
<p>
Assert is a Solidity function, which is recommended to be used only in the development phase. Intentionally, the programmer inserts the function at a specific point in the program where a bug is suspected. If running the program results in gas depletion, the suspicion is confirmed. Thus, this defect refers to the cases in which the assert function is implemented with the wrong purpose, not having the expected effect. In more severe cases, in which the programmer forgets to remove it from the code or does not replace it with require, the impact of this defect can be serious. This vulnerability is also known in the literature as "assert fail" (Zhang et al., 2022a), "assertion failure" (Choi et al., 2021; Torres et al., 2021), "assertion violation" (Sunbeom et al., 2021), or "SWC-110 Assert Violation" (SmartContractSecurity, 2020).</p>


<i>5.4.3 Function Call with Wrong Arguments</i>
<p>This defect refers to the presence of certain control characters within the arguments of a function call, namely the right-to-left override control character, which can cause the function to execute with arguments in reverse order. This is a known issue also in other computing areas (Yosifova and Bontchev, 2021). This vulnerability is also known in the literature as "right to left override" (Tsankov et al., 2018), "rtlo" (Li et al., 2022d), or "SWC-130 Right-To-Left-Override control character (U+202E)"</p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. Now, if encountered, the compiler provides the following informative error message: "Error: Mismatching directional override markers in a comment or string literal"</p>


<h3>5.5 Wrong Class Inheritance Order </h3>
<p>Contracts may have inheritance relationships with other contracts. In the case of solidity, the code of the inherited contract is always executed first, e.g., so that state variables are initialized properly. Solidity uses an algorithm named C3 linearization to determine the order in which the contracts are to be executed. Developers specify the inheritance relationships in a inherit statement and may believe that the order in which the inherited contracts are specified in that statement reflects the
order in which the linearization algorithm should work. This opens space for security issues due to the wrong order of the contract in the inherit statement. This vulnerability is also known in the literature as "SWC-125 Incorrect Inheritance Order"(SmartContractSecurity, 2020).</p>

<p> This issue has been addressed in the latest Solidity compiler, version 0.8.20 at the time of writing. Now, if encountered, the compiler provides the following informative error message: "Error: Derived contract must override function "validPurchase". Two or more base classes define a function with same name and parameter types".</p>

<h3>5.6 Improper Type Usage</h3>
<p>This category groups vulnerabilities in which there is some misuse of types of data structures or functions.</p>

<i>5.6.1 Missing return type on Function</i>
<p>
This vulnerability refers to a missing return type in the definition of a smart contract interface. At runtime, if a contract that implements that interface contains two functions with the same name and arguments but have different return types, there is a chance that the wrong function will be called. This may lead to unexpected results once the calling contract receives the wrong data type (Zhang et al., 2019). This vulnerability is also known in the literature as "ERC 20 standard Violation" (Sunbeom et al., 2021), "ERC20 Interface" (Tsankov et al., 2018), or "missing return statement" (Hu et al., 2023).</p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative error message: "Error: Overriding function return types differ"</p>

<i>5.6.2 Function Return Type Mismatch</i>

<p>In this case, the developer implemented a function (starting from an interface), but it selected the wrong data type for the value to be returned (i.e., it differs from what is specified in the interface). This vulnerability is known in the literature in the context of non-fungible tokens by the name of "ERC721 Interface" (Tsankov et al., 2018) or "SWC-127 Arbitrary Jump with Function Type Variable" (SmartContractSecurity, 2020).</p>


<i>5.6.3 Parameter Type Mismatch</i>
<p>This issue refers to a divergence regarding the types of arguments used in a function that implements an interface. In this situation, even if the call is done with the right function name and arguments, the EVM considers it to be a non-existent function error. This vulnerability is also known in the literature as "ERC20 Indexed" (Tsankov et al., 2018) in the context of fungible tokens. </p>

<i>5.6.4 Missing Type in Variable Declaration</i>

<p>n Solidity, the compiler infers the data type based on the assigned value whenever a variable is declared without an associated type. This additional computation may lead to higher costs (i.e., in gas) and memory usage and especially allows for overflow or underflow problems to occur. For instance, the compiler may infer that a signed integer is the right datatype for a certain variable, where an unsigned integer should be used. This vulnerability is also known in the literature as "unsafe type inference" (Tikhomirov et al., 2018). </p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of
writing. If encountered, the compiler provides the following informative error message: "Error:
Expected primary expression."</p>

<i>5.6.5 Wrong Type in Variable Declaration</i>
<p>
This issue refers to a wrong selection of datatypes that leads to the allocation of more memory than what would be necessary for the intended function, leading to an increase in gas consumption. As an example, in Solidity, the byte[] type reserves 31 bytes of space for each element, whereas the bytes requires a single byte per element, thus being more space efficient. This vulnerability is also known in the literature as "byte array" (Tikhomirov et al., 2018), "global variable" (Li et al., 2022b), "type conversion errors" (Ding et al., 2021), or "unsafe array’s length manipulation" (Shakya et al., 2022).</p>

<i>5.6.6 Wrong Type of Function</i>
<p>
In Solidity, it is possible to specify a type for each function. Functions of type view can read data from state variables but cannot modify them, and no gas costs are involved, whereas functions of type pure neither can read nor modify state variables and similarly to view functions, no gas costs are associated with this type of function. This vulnerability occurs when a developer uses the wrong type for a function. For instance, there is an issue reported in Ethereum’s GitHub (Ethereum’s Github, 2022) in which a state variable conversion operation (from storage to memory) inside a pure function results in a problem (i.e., to avoid this problem, the function type should be view).This vulnerability is also known in the literature as "unsafe type inference" (Tikhomirov et al.,2018) </p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative warning message: "Warning: Function state mutability can be restricted to pure"</p>


<i>Non-Identifiable Order in Map Structure Iteration<i/>
<p> In the Golang language, key-value pairs are not guaranteed to be unique when iterating through a Map structure. This potential lack of uniqueness can cause security issues, particularly if these uncertain values are present in operations that involve modifying the ledger. Such situations may lead to an inconsistent ledger state, which could compromise the ledger’s integrity and reliability. This vulnerability is also known in the literature as "map structure iteration" (Li et al., 2022b)</p>


<h3>5.7 Useless Code</h3>
<p>
This category groups a set of vulnerabilities in which the program contains a unit of code that, in
practice, has no effect. </p>


<i>5.7.1 Unreachable Payable Function</i>
<p> This defect refers to the case of contracts that allow the use of functions that accept credit but do not have any functionality for transacting it. They are insecure, as there is no way to recover the credit once it has been sent to the contract (Zhang et al., 2019). This vulnerability is also known in the literature as "Locked Ether" (Chapman et al., 2019; Ashouri, 2020; Feist et al., 2019; Tsankov et al., 2018), "Lost ether in the transactions" (Argañaraz et al., 2020), "Locked Money" (Zhang et al., 2019; Tikhomirov et al., 2018; Lu et al., 2019), "Frozen Ether" (Hu et al., 2023), "Freezing Ether" (Nguyen et al., 2020; Choi et al., 2021; Jiang et al., 2018; Ashraf et al., 2020), "Be no black hole" (Chang et al., 2019), "Function Can/Cannot Receive Ether" (Chapman et al., 2019), "Locked Funds" (Stephens et al., 2021), "Leaking Ether to Arbitraty Address" (Hu et al., 2023) or "Contracts that lock ether" (Momeni et al., 2019). </p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of
writing. If encountered, the compiler provides the following informative warning message: "Warning:
This function is named receive but is not the receive function of the contract. If you intend this to
be a receive function, use receive(...) ... without the function keyword to define it"</p>

<i>5.7.2 No Effect Code Execution</i>

<p>This vulnerability refers to the presence of code that has no practical purpose (i.e., it has no effect on the intended functionality). Within a smart contract, it increases the size of the program’s binary code, which results in more gas consumption than would otherwise be necessary. This vulnerability is also known in the literature as "call to default constructor" (Tsankov et al., 2018), "dead code" (Chen et al., 2021), "useless assignment(Hu et al., 2023)", "code-no-effects" (Li et al., 2022d), or "SWC-135 Code With No Effects" (SmartContractSecurity, 2020).</p>

<i>5.7.3 Unused Variables</i>
<p>
This defect refers to the declaration of variables that are not used in the contract, which results directly in the allocation of unnecessary space in memory. As a consequence, the gas cost of executing the contract increases as well as the attack surface of the contract. Other effects are related to the readability or maintainability of the code. This vulnerability is also known in the literature as "redundant sstore" (Chen et al., 2021), "unused state variable" (Hu et al., 2023), "unused State Variable" (Tsankov et al., 2018), or "SWC-131 Presence of unused variables" (SmartContractSecurity, 2020). </p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative warning message: "Warning: Unused local variable. Warning: Unused function parameter. Remove or comment out the variable name to silence this warning"</p>


<i>5.7.4 Inefficient Operation Sequence</i>

<p> 
  Due to bad programming practices or outdated compilers (i.e., Solidity has more than 350 versions, with the first one being 0.1.1 released in 2015), smart contracts may suffer from gas-inefficient operation sequences. Consequently, such contracts could be deployed with non-optimized bytecode, leading to increased resource and gas consumption. This vulnerability is also known in the literature as "SWAP1=DUP2=SWAP1" (Chen et al., 2021), "PUSHx=POP" (Chen et al., 2021), or"PUSH1=NOT" (Chen et al., 2021) </p>


<h3>5.8 Version Issues</h3>
<p>
This category refers to issues that relate to the versioning of various aspects, including the use of deprecated versions of functions. </p>

<i>5.8.1 Undetermined Program Version Prevalence</i>

<p>This defect refers to the case where the developer allows a certain contract to be compiled across multiple versions. This allows the known faults in older versions to be easily activated. (Zhang et al., 2019). This vulnerability is also known in the literature as "compiler version not fixed" (Tikhomirov et al., 2018), "Solc Version" (Tsankov et al., 2018), or "SWC-103 Floating Pragma" (SmartContractSecurity, 2020) </p>


<i>5.8.2 Outdated Compiler Version</i>
<p>
Contracts that have been developed against an outdated compiler version can bring in several risks, mainly because newer versions may have resolved certain bugs or even introduced security mechanisms to avoid particular issues (e.g., the throw function has been disallowed in Solidity 0.5.0 and superior versions, in favor of assert, require⁄, and revert). This vulnerability is also known in the literature as "SWC-102 Outdated Compiler Version" (SmartContractSecurity, 2020).</p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative error message: "Error: Source file requires different compiler version (current compiler is 0.8.20+commit.a1b79de6.Darwin.appleclang)- note that nightly builds are considered to be strictly less than the released version"</p>

<i>5.8.3 Use of Deprecated Functions</i>
<p>
Deprecated functions are not recommended due to the fact that they are usually replaced by functions that solve known security issues or even operate in a more efficient manner (i.e., may consume less gas). As an example, sha3 was marked as a deprecated function in Solidity 0.5 and replaced by keccak256, which is more secure and efficient. This vulnerability is also known in the literature as "SWC-111 Use of deprecated solidity functions" (SmartContractSecurity, 2020).</p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative error message: "Error: suicide has been deprecated in favor of self-destruct"</p>

<h3>5.9 Inadequate Data Representation</h3>
<p>
The numbers to represent the credits (e.g., Ether) can be very large (i.e., literals with many digits are difficult to read and review). Thus it is recommended that the programmer use the native resources of the language to make this representation (e.g., Solidity 10000000000000000000 for 1 ether). This vulnerability is also known in the literature as "too-many-digits" (Li et al., 2022d; Tsankov et al., 2018) </p>

<h3>5.10 Improper Modifier</h3>
<p>This group gathers defects that relate to the use of modifiers in functions and variables.</p>

<i>5.10.1 Wrong Function Modifier</i>
<p>
This defect refers to the case of functions that are written solely to be used by other contracts (i.e., not within the contract). Such functions should be marked with the external modifier instead of public. The public modifier allows both external and internal calls. Marking a function with external results in gas savings, as every invocation will be using calldata (a special memory region to store arguments, which cannot be later modified by the function) and can avoid unnecessary read and write operations to memory, which occur with internal calls (i.e., that do not use calldata). This vulnerability is also known in the literature as "external-function" (Tsankov et al., 2018), or "SWC-100: Function Default Visibility" (SmartContractSecurity, 2020). </p>

<i>5.10.2 Missing Constant Modifier in Variable Declaration</i>
<p>
Variables that are not modified during the execution flow should be declared as constants to save gas. In the absence of the constant modifier, it is assumed that the variable’s value can be changed. This vulnerability is also known in the literature as "constable states" (Tsankov et al., 2018). </p>


<i>5.10.3 Missing Visibility Modifier in Variable Declaration</i>
<p>
Variables have different visibility states, which determine the context for accessing them. In Solidity,by default, the visibility of state variables and functions is internal, which allows access from functions in the same contract or derived contracts. A developer that is unaware of this may create a contract that allows exposure of sensitive data or allow unexpected behavior. This vulnerability is also known in the literature as "implicit Visibility" (Ashizawa et al., 2021) , "state variables default
visibility" (Tsankov et al., 2018), "visibility level" (Tikhomirov et al., 2018), or "SWC-108: State Variable Default Visibility" (SmartContractSecurity, 2020). </p>

<h3>5.11 Redundant Functionality</h3>
<p>
Contracts that are written with redundant functionality increase code size and make maintainability difficult. In a simple scenario, a programmer creates a function and later (by bad practices) ends up creating the same functionality again in a new function. He/she identifies a vulnerability in the new function and fixes it, but the old function with the defect is used by the caller. This vulnerability is also known in the literature as "redundant fallback function" (Shakya et al., 2022) or "redundant
fallback function" (Tikhomirov et al., 2018). </p>

<h3>5.12 Shadowing</h3>
<p> This category groups defects in which there are code elements (e.g., a function or a variable) with the same name, which can lead to erroneous and unexpected behavior.</p>

<i>5.12.1 Use of Same Variable or Function Name In Inherited Contract</i>

<p>
  When using the same name as a local variable, which was previously declared by an inherited contract, the program loses the reference of the inherited variable, causing the local variable to assume the role of the other variable. This vulnerability is also known in the literature as "shadow memory" (Ashouri, 2020), "shadowing state variables" (Tsankov et al., 2018), "shadowing" (Feist et al., 2019), or "SWC-119: Shadowing State Variables" (SmartContractSecurity, 2020). </p>

<p> This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative error message: "Error: Identifier already declared".</p>

<i>5.12.2 Variables or Functions Named After Reserved Words</i>

<p>This bug occurs when creating variables named after keywords of the language itself. For example, in Solidity, creating a variable with the name now conflicts with the function that returns the date and time. This vulnerability is also known in the literature as "shadowed builtin(Tsankov et al., 2018) or "shadowing-builtin" (Li et al., 2022d) </p>

<p> This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative warning message: "Warning: This declaration shadows a built-in symbol"
 </p>


<i>5.12.3 Use of the Same Variable or Function Name In a Single Contract</i>
<p>
This vulnerability refers to cases where the same name is used for more than one variable or function inside the contract. This makes the program lose the reference of the variable of the class, assuming the variable of the function as its role. This vulnerability is also known in the literature as "redefined variable" (Hu et al., 2023) or "shadowed local variable" (Tsankov et al., 2018)</p>

<p>This issue has been addressed in the latest Solidity compiler, version 0.8.20, at the time of writing. If encountered, the compiler provides the following informative warning message: "Warning: This declaration shadows an existing declaration"</p>


<h3>5.13 Buffer Overflow</h3>
<p>
This category refers to buffer-related vulnerabilities (e.g., stack-based, heap-based, bugger overread) in which it is possible to write more data than what the buffer can hold, thus modifying memory areas outside the expected or read the buffer using mechanisms such as indexes or pointers that reference memory locations after the targeted buffer. </p>

<i>5.13.1 Stack-based Buffer Overflow</i>
<p>
The EVM keeps an execution stack that manages the execution of contracts. If an attacker is allowed to overflow this stack (e.g., by using specially crafted inputs), it can potentially overwrite control variables (e.g., timestamp or block number) and, for instance, gain unauthorized access to certain resources. This vulnerability is also known in the literature as "Stack size limited" (Atzei et al., 2017). </p>

<i>5.13.2 Write to Arbitrary Storage Location</i>
<p>
In solidity, arrays are stored as contiguous fixed-size slots. In the absence of a bounds verification, a malicious user could write data to a particular storage slot used to store the contract owner’s address, which could be overwritten and then used to further harm the contract. This vulnerability is also known in the literature as "arbitrary write" (Choi et al., 2021), "buffer-overwrite" (Pani et al., 2023), "storage modification" (Krupp and Rossow, 2018), "unrestricted write" (Tsankov et al., 2018), or "SWC-124: Write to Arbitrary Storage Location" (SmartContractSecurity, 2020).. </p>

<h3>4.14 Use of Malicious Libraries</h3>
<p> This defect refers to the use of third-party libraries containing malicious code. This vulnerability is
also known in the literature as "Malicious libraries" (Tikhomirov et al., 2018), "Unknown libraries"
(Lu et al., 2019), or "Dynamic libraries" (Andesta et al., 2020). </p> 

<h3>4.15 Typographical Error</h3>
<p>
This defect refers to single-digit errors made by programmers while typing source code, e.g., in logic
or arithmetic operations. For example, for value assignment, a developer may type by mistake "+ ="
instead of "=" or may use "−" instead of "+" or "−−" instead of "++" (Hartel and Schumi, 2020).
This vulnerability is also known in the literature as "Assignment operator replacement", "Binary
operator replacement", "Unary operator replacement or deletion" (Hartel and Schumi, 2020), or
"SWC-129 Typographical Error" (SmartContractSecurity, 2020). </p> 




<h2>5. Incorrect Control Flow</h2>
<p>
This category groups a set of vulnerabilities that, if exploited, cause changes in the control flow of
the program.</p>

<h3>5.1 Incorrect Sequencing of Behavior</h3>
<p>
This category gathers vulnerabilities that end up in a sequence of behaviors that are carried out in
the wrong order, leading to unexpected results. </p>

<i>5.1.1 Incorrect Use of Event Blockchain variables for Time</i>
<p>
Contracts that rely on using block control information (i.e., timestamp, coinbase, number, diffi-
culty, and gas limit) for sequential event control are vulnerable to tampering by the miner. This
vulnerability is also known in the literature as "Block state dependence" (Kalra et al., 2018), "Time
restrictions" (Argañaraz et al., 2020), "Blockchain effects time dependency" (Zhang et al., 2020),
"Block State Dependency" (Choi et al., 2021), "Timestamp dependence" (Ma et al., 2022; Hu et al.,
2023; Song Jingjing and He et al., 2019; Wang et al., 2021; Andesta et al., 2020; Akca et al., 2019;
Feng et al., 2019; Luu et al., 2016; Nguyen et al., 2020; Ashraf et al., 2020; Chen et al., 2020; Jiang
et al., 2018; Tikhomirov et al., 2018; Zhang et al., 2019), "Block number dependency" (Chen et al.,
2020; Jiang et al., 2018), "BlockTimestamp" (Liao et al., 2019), "Time dependency" (Liao et al.,
2019), "Race condition" (Ashouri, 2020) "Block No. Dependency" (Ashraf et al., 2020), "Block
number dependency" (Nguyen et al., 2020), "Event-ordering (EO) bugs" (Kolluri et al., 2019) or
"SWC-116: Block values as a proxy for time" (SmartContractSecurity, 2020).</p>

<i>5.1.2 Incorrect Function Call Order</i>
<p>
This defect refers to the creation of public functions that expect to be called in a certain sequence,
originating unanticipated results whenever clients do not follow the right call order (Mavridou
Anastasia et al., 2018). This vulnerability is also known in the literature , "Transaction-ordering
dependence" (Mavridou Anastasia et al., 2018) or "SWC-114: Transaction Order Dependence"
(SmartContractSecurity, 2020).</p>

<i>5.1.3 Improper Locking</i>
<p>
This issue refers to the case where a contract assumes that all entities participating in a transaction
must have the same credit balance before the contract operations can execute. If there are no
adequate (e.g., wrong or even missing) locking mechanisms, an attacker can forcefully send credit to
the other entity, which would cause the verification of the balance condition to never be met. Thus,
the contract may become unusable or show unexpected behavior (or unexpected state changes).
This vulnerability is also known in the literature as "IncorrectEquality" (Tsankov et al., 2018),
"Balance equality" (Tikhomirov et al., 2018; Zhang et al., 2019), "Strict equality" (Lu et al., 2019),
"Strict Check for Balance" (Chen et al., 2020), "Arbitrary sending of ether" (Feist et al., 2019) or
"SWC-132: Unexpected Ether balance" (SmartContractSecurity, 2020).</p>

<i>5.1.4 Transfer Pre-Condition Dependent on Transaction Order</i>
<p>
In the case of this vulnerability, the order in which transactions are executed influence a pre-
condition that guards the execution of the transfer. This influence may erroneously result in, for
instance, a transaction not being executed at all. This defect is known in the literature as TOD-
Transfer (Tsankov et al., 2018), TOD (Bose et al., 2022) or Transaction Order Dependence (Smart-
ContractSecurity, 2020).</p>

<i>5.1.5 Transfer Amount Dependent on Transaction Order</i>
<p>
This issue refers to the case where the value of the variable that stores or determines an amount
of a digital asset (to be transferred) is modified before it is sent to the recipient due to transaction
ordering within a block. The amount may be changed due to the effect of multiple transactions being
grouped in a block and executed in a specific order having the effect of producing unexpected changes
in the value being transferred. This vulnerability is also known in the literature as "TODAmount"
(Tsankov et al., 2018) , "TOD" (Liao et al., 2019; Wang et al., 2021; Bose et al., 2022) or "SWC-114:
Transaction Order Dependence" (SmartContractSecurity, 2020).</p>

<i>5.1.6 Transfer Recipient Dependent on Transaction Order</i>
<p>In the case of this defect, the transfer recipient is modified before the send event due to transaction
ordering within a block. As an example, if the intended recipient address is stored as a storage
variable and a transfer is to execute based on this address, there is a chance the address may be
changed or overwritten by another transaction prior to the transfer. This vulnerability is also known
in the literature as "TODReceiver" (Tsankov et al., 2018) , "Direct value transfer" (Krupp and
Rossow, 2018), "Transaction order dependence" (Kalra et al., 2018; Hu et al., 2023), "Transaction-
ordering dependence" (Song Jingjing and He et al., 2019; Grishchenko et al., 2018; Luu et al., 2016;
Grieco et al., 2020), "TOD" (Bose et al., 2022) or "SWC-114: Transaction Order Dependence"
(SmartContractSecurity, 2020).</p>

<i>5.1.7 Exposed state variables</i>
<p> This vulnerability refers to the case where a developer erroneously exposes a state variable, whose
value may then be modified by an attacker so that this modification influences the execution of
a certain contract operation. As an example, consider a contract that executes a credit transfer
from one user to another and has a require statement for verifying that there is sufficient credit
to conclude the operation. If the balance is stored as a public state variable, a malicious use could
change its value so that the require is avoided allowing the user to run a transfer that exceeds
the amount of credit actually held by the malicious user. This vulnerability is also known in the
literature as "Vulnerable state" (Krupp and Rossow, 2018). </p>

<h3>5.2 Inadequate Input Validation</h3>
<p>
This group refers to defects involving the inadequate validation of functional conditions, which are
requirements that a contract must meet so that it can operate correctly. Such conditions may offer
protection against certain types of attacks or force certain business rules to be followed.</p>

<i>5.2.1 Improper Input Validation</i>
<p> This type of problem occurs when an attacker calls a certain contract operation using invalid or
malicious input data, capable of affecting the functioning of the contract due to the fact that either
it does not validate the incoming inputs or validates them in an incorrect manner. For instance, in
the context of Solidity, a Short Address Attack occurs when a contract receives less data than it
was expecting, which leads the system to fill the missing bytes with zeros (Chen et al., 2020). As
a consequence, the behavior may become unexpected if the code assumes that the input data will
comply with a certain length or format. This vulnerability is also known in the literature as "Invalid
input data" (Chen et al., 2020; Grieco et al., 2020), "Short address attack" (Ashouri, 2020), "Short
address" (Feng et al., 2019), or "Avoid non-existing address" (Chang et al., 2019). </p>

<i>5.2.2 Extraneous Input Validation</i>
<p> In this particular case, the functional conditions of the contract are too strong and do not allow
certain behaviors (which would be valid) to occur, making the contract unable to meet the require-
ments. This vulnerability is also known in the literature as "Requirement Violation" (Choi et al.,
2021) or "SWC-123 Requirement violation" (SmartContractSecurity, 2020). </p>

<h2>6. Arithmetic Issues</h2>
<p> This category groups different vulnerabilities that share the outcome of resulting in arithmetic
problems. </p>

<h3>6.1 Overflow and Underflow</h3>
<p> This category refers to the use of operations (e.g., addition, subtraction) over values that result in
a value that is less than (or greater than) the minimum values (or maximum value) that a variable
can hold, which produces a value different from the correct result. </p>

<i>6.1.1 Integer Underflow</i>
<p>This defect refers to operations over an Integer variable that result in a value that is less than the
minimum value allowed by the Integer type. This vulnerability is also known in the literature as
"Integer Bug" (Choi et al., 2021), "Integer underflow" (Stephens et al., 2021; Song Jingjing and He
et al., 2019; Momeni et al., 2019; Wang et al., 2021; Kalra et al., 2018), "Integer overflow/underflow"
(Wang et al., 2019; Lu et al., 2019; So et al., 2020), "Integer overflow and integer underflow" (Ayoade
et al., 2019), "Arithmetic bugs" (Torres et al., 2018), "Underflow" (Liao et al., 2019), "Integer
overflow and underflow" (Nguyen et al., 2020; Ashouri, 2020), "Overflow/Underflow" (Ma et al.,
2022; Cui et al., 2022; Hu et al., 2023; Akca et al., 2019), "Arithmetic issues" (Andesta et al., 2020)
or "SWC-101: Integer Overflow and Underflow" (SmartContractSecurity, 2020).</p>

<i>6.1.2 Integer Overflow</i>
<p>This defect refers to operations over an Integer variable that results in a value that is larger than
the maximum value allowed by the Integer type. This vulnerability is also known in the literature
as "Integer Bug" (Choi et al., 2021), "Integer overflow" (Stephens et al., 2021; Wang et al., 2021;
Kalra et al., 2018), "Integer overflow/underflow" (Ma et al., 2022; Cui et al., 2022; Hu et al., 2023;
So et al., 2020; Wang et al., 2019), "Integer overflow and integer underflow" (Ayoade et al., 2019),
"Integer overflows" (Grieco et al., 2020; Grech et al., 2020), "Arithmetic Bugs" (Torres et al.,
2018), "Overflow detector" (Gao et al., 2019), "Overflow" (Liao et al., 2019), "Integer overflow
and underflow" (Nguyen et al., 2020; Ashouri, 2020), "BatchOverflow" (Feng et al., 2019), "Over-
flow/Underflow" (Akca et al., 2019), "Arithmetic issues" (Andesta et al., 2020), "Integer Overflow
vulnerability" (Song Jingjing and He et al., 2019) or "SWC-101: Integer Overflow and Underflow"
(SmartContractSecurity, 2020). </p>

<h3>6.2 Division Bugs</h3>
<p>This category groups issues related to erroneous division operations.</p>

<i>6.2.1 Divide by Zero</i>
<p> This issue refers to the attempt of a program to divide a value by zero. This vulnerability is also
known in the literature as "Division-by-zero" (So et al., 2020), "Arithmetic Bugs" (Torres et al.,
2018), or "Division by zero" (Akca et al., 2019). </p>

<i>6.2.2 Integer Division</i>
<p>At the time of writing, a smart contract mainstream language like Solidity does not support floating
point or decimal types. Thus, the remainder of a division operation is always lost. Developers may
use fixed-point arithmetic and external libraries to handle this kind of operation. This vulnerability
is also known in the literature as "Numerical Precision Error" (Cui et al., 2022), "Integer division"
(Tikhomirov et al., 2018) , "Using fixed point number type" (Zhang et al., 2019) or "SWC-101:
Integer Overflow and Underflow" (SmartContractSecurity, 2020).</p>

<h3>6.3 Conversion Bugs</h3>
<p> This category groups a set of vulnerabilities where there are issues related to the conversion between
different datatypes.</p>

<i>6.3.1 Truncation Bugs</i>
<p> This vulnerability refers to the case where a variable declared in a certain type is converted to a
smaller type, which means that data is lost during the conversion process. This vulnerability is also
known in the literature as "Truncation bugs" (Torres et al., 2018) or "SWC-101: Integer Overflow
and Underflow" (SmartContractSecurity, 2020). </p>


<i>6.3.2 Signedness Bugs</i>
<p>The conversion of a signed integer type to an unsigned type of the same width may change a negative
value to a positive one (the opposite may also happen) (Torres et al., 2018). This vulnerability is also
known in the literature as "Signedness bugs" (Torres et al., 2018) or "SWC-101: Integer Overflow
and Underflow" (SmartContractSecurity, 2020). </p>

<h2>7. Improper Access Control</h2>
<p> This category groups a set of vulnerabilities that are strongly related to authentication or access
control. </p>

<h3>7.1 Incorrect Authentication or Authorization</h3>
<p>The smart contract fails to properly identify a client or determine its privileges, resulting in wrong
access privileges for that particular client.</p>

<i>7.1.1 Wrong Caller Identification</i>
<p> In Solidity, tx.origin allows obtaining the address of the account that initiated a transaction
and msg.sender allows obtaining the address of the contract that has called the function being
executed. The use of the tx.origin for access control may be a way of opening an entry point to
a malicious user. A malicious user may create a contract that calls the vulnerable function (i.e.,
the one that uses tx.origin to check the identity of the caller). Thus, msg.sender will differ from
tx.origin. In the case the vulnerable function uses tx.origin for access control, it will allow the
user to perform actions it should not be able to. This vulnerability is also known in the literature
as "Transaction Origin Use" (Choi et al., 2021), "Transaction state dependence" (Kalra et al.,
2018), "Use of origin" (Brent et al., 2018), "TxOrigin" (Hu et al., 2023; Li et al., 2023; Akca et al.,
2019; Liao et al., 2019; Tsankov et al., 2018), "Tx.origin for authentication" (Zhang et al., 2019),
"Tx.origin" (Lu et al., 2019; Tikhomirov et al., 2018), "Incorrect Check for Authorization" (Chen
et al., 2020), "Unprotected usage of tx.origin" (Momeni et al., 2019) or "SWC-115: Authorization
through tx.origin" (SmartContractSecurity, 2020). </p>

<i>7.1.2 Owner Manipulation</i>
<p> This vulnerability allows an attacker to exploit some function or feature of the smart contract by
manipulating the owner control variable. This allows the attacker to perform some kind of restricted
operations (Zhang et al., 2020). This vulnerability is also known in the literature as "Missing Owner
Check" (Cui et al., 2022), "Unprotected Function" (Stephens et al., 2021), "Vulnerable access
control" (Zhang et al., 2020), "Access control" (Feng et al., 2019; Lu et al., 2019), or "Tainted
owner variable" (Brent et al., 2020) </p>


<i>7.1.3 Missing Verification for Program Termination</i>
<p> This issue refers to the lack of a secure verification for terminating a published (deployed) con-
tract, allowing an attacker to terminate it in an unauthorized manner. Selfdestruct is an EVM
instruction that is able to nullify the bytecode of a deployed contract. When invoked, it stops the
execution of the EVM, deletes the contract’s bytecode, and sends the remaining fund to a certain
address. Access to this kind of function by non-authorized clients may result in security issues.
This vulnerability is also known in the literature as "Suicidal Contract" (Choi et al., 2021), "Un-
protected Suicide" (Hu et al., 2023), "Destroyable contract" (Brent et al., 2018), "SelfDestruct"
(Liao et al., 2019), "Suicidal contracts" (Feist et al., 2019), "Guard suicide" (Chang et al., 2019),
"Unprotected usage of selfdestruct" (Momeni et al., 2019), "Accessible selfdestruct" (Brent et al.,
2020), "Tainted selfdestruct" (Brent et al., 2020) or "SWC-106: Unprotected SELFDESTRUCT
Instruction" (SmartContractSecurity, 2020). </p>

<h3>7.2 Improper Protection of Sensitive Data</h3>
<p>  This category generally refers to the issues that result in the inability to protect sensitive information
from non-authorized clients. </p>

<i>7.2.1 Exposed Private Data</i>
<p> This issue refers to the cases in which contracts store unencrypted sensitive data in public blockchain
transactions. Solidity, like other programming languages, support the private keyword that indi-
cates that data is only accessible within the contract itself. However, in blockchain environments,
marking a variable with private does not make it fully invisible to the outside world. Miners, which
are responsible for validating transactions on the blockchain, can view the code of the contract and
the value of its state variables (Zhang et al., 2019). This vulnerability is also known in the litera-
ture as "Keeping Secrets" (Argañaraz et al., 2020), "Exposed secret" (Zhang et al., 2020), "Private
modifier" (Lu et al., 2019; Tikhomirov et al., 2018; Zhang et al., 2019) or "SWC-136: Unencrypted
Private Data On-Chain" (SmartContractSecurity, 2020). </p>

<i> 7.2.2 Dependency on External State Data (Unsolvable constraints of external critical state data) </i>
<p> This vulnerability refers to the use of data that is not under control nor is generated by the contract
(i.e., external critical state data). A malicious user may exploit this situation if such data determines
the outcome of the execution of the contract. This vulnerability is also known in the literature as
"Unsolvable constraints" (Zhang et al., 2020). </p>

<h3> 7.3 Cryptography Misuse </h3>
<p> This category groups vulnerabilities that generally reflect misuse of cryptography mechanisms. </p>

<i>7.3.1 Incorrect Verification of Cryptographic Signature</i>
<p>
This issue refers to the wrong verification of the authenticity and integrity of messages with the use
of message signatures. As an example, a developer could develop a vulnerable contract that relies on
a signature in a signed message hash for representing the earlier verification of previous messages.
A client could generate a malicious message with a valid signature and include it in the hash. The
contract then would validate the signature and update the hash, indicating that the message was
processed. This vulnerability is also known in the literature as "Missing Key Check" (Cui et al.,
2022), "SWC-117: Signature Malleability" (SmartContractSecurity, 2020). </p>

<i>7.3.2 Improper Check against Signature Replay Attacks</i>
<p>This defect refers to a situation where a malicious client is able to obtain the message hash of a
legitimate transaction and is allowed to use the same signature to impersonate the legitimate client
and execute fraudulent transactions. This vulnerability is also known in the literature as "SWC-121:
Missing Protection against Signature Replay Attacks" (SmartContractSecurity, 2020). </p>

<i>7.3.3 Improper Authenticity Check</i>
<p> In this case, a contract may tolerate off-chain signed messages instead of waiting for an on-chain
signature. This is usually done with the goal of improving performance but may come at the expense
of compromising the authenticity of the message. This vulnerability is also known in the literature
as "Missing Signer Check" (Cui et al., 2022), "SWC-122: Lack of proper signature verification"
(SmartContractSecurity, 2020). </p>

<i>7.3.4. Incorrect Argument Encoding</i>
<p> This defect refers to the misuse of one-way hash functions (i.e., Solidity keccak256) namely in
the incorrect encoding of the function arguments, which can result in a higher likelihood of hash
collisions for different entries. This vulnerability is also known in the literature as "Authorization"
(Mavridou Anastasia et al., 2018) , "Hash collision" (Lu et al., 2019) or "SWC-133: Hash Collisions
With Multiple Variable Length Arguments" (SmartContractSecurity, 2020).</p>


<h2> References </h2>

Agbo C, Mahmoud Q, Eklund J (2019) Blockchain Technology in Healthcare: A Systematic Review.
Healthcare 7(2):56, DOI 10.3390/healthcare7020056, URL https://www.mdpi.com/2227-9032/
7/2/56
<br>

Akca S, Rajan A, Peng C (2019) SolAnalyser: A Framework for Analysing and Testing Smart
Contracts. In: 2019 26th Asia-Pacific Software Engineering Conference (APSEC), IEEE, Putra-
jaya, Malaysia, pp 482–489, DOI 10.1109/APSEC48747.2019.00071, URL https://ieeexplore.ieee.org/document/8945725/
<br>

Amiet N (2021) Blockchain Vulnerabilities in Practice. Digital Threats: Research and Practice
2(2):1–7, DOI 10.1145/3407230
<br>

Amoroso EG (1994) Fundamentals of computer security technology. Prentice-Hall, Inc., USA
<br>

Andesta E, Faghih F, Fooladgar M (2020) Testing Smart Contracts Gets Smarter. In: 2020 10th
International Conference on Computer and Knowledge Engineering (ICCKE), IEEE, Mashhad,
Iran, pp 405–412, DOI 10.1109/ICCKE50421.2020.9303670, URL https://ieeexplore.ieee.
org/document/9303670/
<br>

di Angelo M, Salzer G (2019) A Survey of Tools for Analyzing Ethereum Smart Contracts. In:
2019 IEEE International Conference on Decentralized Applications and Infrastructures (DAPP-
CON), IEEE, Newark, CA, USA, pp 69–78, DOI 10.1109/DAPPCON.2019.00018, URL https://ieeexplore.ieee.org/document/8782988/
<br>

Antonopoulos A, Wood G (2018) Mastering Ethereum: Building Smart Contracts and DApps.
O’Reilly Media, Inc.
<br>

Argañaraz MC, Berón MM, Pereira MJV, Henriques PR (2020) Detection of Vulnerabilities in Smart
Contracts Specifications in Ethereum Platforms. In: 9th Symposium on Languages, Applications
and Technologies (SLATE 2020), Schloss Dagstuhl–Leibniz-Zentrum für Informatik, Barcelos,
Portugal, OpenAccess Series in Informatics (OASIcs), p 16, DOI 10.4230/OASIcs.SLATE.2020.0
<br>

Ashouri M (2020) Etherolic: a practical security analyzer for smart contracts. In: Proceedings of the
35th Annual ACM Symposium on Applied Computing, Association for Computing Machinery,
pp 353–356, DOI 10.1145/3341105.3374226, URL https://dl.acm.org/doi/10.1145/3341105.3374226.
<br>

Ashraf I, Ma X, Jiang B, Chan WK (2020) GasFuzzer: Fuzzing Ethereum Smart Contract Binaries
to Expose Gas-Oriented Exception Security Vulnerabilities. IEEE Access 8:99552–99564, DOI 10.1109/ACCESS.2020.2995183
<br>

Atzei N, Bartoletti M, Cimoli T (2017) A Survey of Attacks on Ethereum Smart Contracts (SoK).
pp 164–186, DOI 10.1007/978-3-662-54455-6_8
<br>

Avizienis A, Laprie JC, Randell B, Landwehr C (2004) Basic concepts and taxonomy of dependable
and secure computing. IEEE Transactions on Dependable and Secure Computing 1(1):11–33,
DOI 10.1109/TDSC.2004.2, URL http://ieeexplore.ieee.org/document/1335465/
<br>

Ayoade G, Bauman E, Khan L, Hamlen K (2019) Smart Contract Defense through Bytecode Rewrit-
ing. In: 2019 IEEE International Conference on Blockchain (Blockchain), IEEE, Atlanta, GA,
USA, pp 384–389, DOI 10.1109/Blockchain.2019.00059, URL https://ieeexplore.ieee.org/document/8946210/
<br>

Bishop M, Bailey D (1996) A Critical Analysis of Vulnerability Taxonomies. Tech. rep., URL https://apps.dtic.mil/sti/citations/ADA453251
<br>

Blockstack, Algorand (2021) Clarity. URL https://github.com/clarity-lang
<br>

Bose P, Das D, Chen Y, Feng Y, Kruegel C, Vigna G (2022) SAILFISH: Vetting Smart Contract
State-Inconsistency Bugs in Seconds. In: 2022 IEEE Symposium on Security and Privacy (SP), IEEE, pp 161–178, DOI 10.1109/SP46214.2022.9833721
<br>

Brent L, Jurisevic A, Kong M, Liu E, Gauthier F, Gramoli V, Holz R, Scholzm B (2018) Vandal: A
scalable security analysis framework for smart contracts. URL https://arxiv.org/pdf/1809.03981v1.pdf
<br>

Brent L, Grech N, Lagouvardos S, Scholz B, Smaragdakis Y (2020) Ethainter: A Smart Contract
Security Analyzer for Composite Vulnerabilities. In: Proceedings of the 41st ACM SIGPLAN
Conference on Programming Language Design and Implementation, Association for Computing
Machinery, New York, NY, USA, PLDI 2020, pp 454–469, DOI 10.1145/3385412.3385990, URL
https://doi.org/10.1145/3385412.3385990
<br>

Chang J, Gao B, Xiao H, Sun J, Cai Y, Yang Z (2019) sCompile: Critical Path Identification
and Analysis for Smart Contracts. In: Ait-Ameur Y, Qin S (eds) Formal Methods and Software
Engineering, Springer International Publishing, Cham, pp 286–304
<br>

Chapman P, Xu D, Deng L, Xiong Y (2019) Deviant: A Mutation Testing Tool for Solidity Smart
Contracts. In: 2019 IEEE International Conference on Blockchain (Blockchain), IEEE, Atlanta,
GA, USA, pp 319–324, DOI 10.1109/Blockchain.2019.00050, URL https://ieeexplore.ieee.org/document/8946219/
<br>

Chen T, Cao R, Li T, Luo X, Gu G, Zhang Y, Liao Z, Zhu H, Chen G, He Z, Tang Y, Lin X, Zhang
X (2020) SODA: A Generic Online Detection Framework for Smart Contracts. In: Proceedings
2020 Network and Distributed System Security Symposium, Internet Society, Reston, VA, DOI 10.
14722/ndss.2020.24449, URL https://www.ndss-symposium.org/wp-content/uploads/2020/02/24449.pdf
<br>

Chinen Y, Yanai N, Cruz JP, Okamura S (2020) RA: Hunting for Re-Entrancy Attacks in Ethereum
Smart Contracts via Static Analysis. In: 2020 IEEE International Conference on Blockchain
(Blockchain), IEEE, Rhodes, Greece, pp 327–336, DOI 10.1109/Blockchain50366.2020.00048,URL https://ieeexplore.ieee.org/document/9284679/
<br>

Choi J, Kim D, Kim S, Grieco G, Groce A, Cha SK (2021) SMARTIAN: Enhancing Smart Con-
tract Fuzzing with Static and Dynamic Data-Flow Analyses. In: 2021 36th IEEE/ACM In-
ternational Conference on Automated Software Engineering (ASE), IEEE, pp 227–239, DOI 10.1109/ASE51524.2021.9678888
<br>

Coblenz M (2019) The Obsidian Smart Contract Language. URL https://obsidian.readthedocs.io/en/latest/
<br>

ConsenSys (2021) Mythril. URL https://github.com/ConsenSys/mythril
<br>

Cui S, Zhao G, Gao Y, Tavu T, Huang J (2022) VRust. In: Proceedings of the 2022 ACM SIGSAC
Conference on Computer and Communications Security, ACM, New York, NY, USA, pp 639–652,
DOI 10.1145/3548606.3560552, URL https://dl.acm.org/doi/10.1145/3548606.3560552
<br>

CWE Community (2009) Common Weakness Enumeration. URL https://cwe.mitre.org/about/index.html
<br>

Durieux T, Ferreira JF, Abreu R, Cruz P (2020) Empirical Review of Automated Analysis Tools
on 47,587 Ethereum Smart Contracts. In: Proceedings of the ACM/IEEE 42nd International
Conference on Software Engineering, Association for Computing Machinery, New York, NY,
USA, ICSE ’20, pp 530–541, DOI 10.1145/3377811.3380364, URL https://doi.org/10.1145/3377811.3380364
<br>

Ethereum’s Github (2022) Pure Issue. URL https://github.com/ethereum/solidity/issues/13174
<br>

Feist J, Grieco G, Groce A (2019) Slither: A Static Analysis Framework for Smart Contracts.
In: 2019 IEEE/ACM 2nd International Workshop on Emerging Trends in Software Engineering for Blockchain (WETSEB), IEEE, Montreal, QC, Canada, WETSEB ’19, pp 8–15, DOI
10.1109/WETSEB.2019.00008, URL https://doi.org/10.1109/WETSEB.2019.00008https://
ieeexplore.ieee.org/document/8823898/
<br>

Feng Y, Torlak E, Bodik R (2019) Precise Attack Synthesis for Smart Contracts, URL http://arxiv.org/abs/1902.06067
<br>

Gao J, Liu H, Liu C, Li Q, Guan Z, Chen Z (2019) EASYFLOW: Keep Ethereum Away from
Overflow. In: 2019 IEEE/ACM 41st International Conference on Software Engineering: Com-
panion Proceedings (ICSE-Companion), IEEE, Montreal, QC, Canada, pp 23–26, DOI 10.1109/
ICSE-Companion.2019.00029, URL https://ieeexplore.ieee.org/document/8802775/
<br>

Geneiatakis D, Soupionis Y, Steri G, Kounelis I, Neisse R, Nai-Fovino I (2020) Blockchain Per-
formance Analysis for Supporting Cross-Border E-Government Services. IEEE Transactions
on Engineering Management 67(4):1310–1322, DOI 10.1109/TEM.2020.2979325, URL https:
//ieeexplore.ieee.org/document/9102377/
<br>

Ghaleb A, Pattabiraman K (2020) How Effective Are Smart Contract Analysis Tools? Evaluating
Smart Contract Static Analysis Tools Using Bug Injection. In: Proceedings of the 29th ACM
SIGSOFT International Symposium on Software Testing and Analysis, Association for Comput-
ing Machinery, New York, NY, USA, ISSTA 2020, pp 415–427, DOI 10.1145/3395363.3397385,
URL https://doi.org/10.1145/3395363.3397385 government U (1999) National Vulnerability Database. URL https://nvd.nist.gov/
<br>

Grech A, Camilleri AF (2017) Blockchain in Education. Publications Office of the European Union,
DOI 10.2760/60649, URL https://ec.europa.eu/jrc/en/open-education/legal-notice
<br>

Grech N, Kong M, Jurisevic A, Brent L, Scholz B, Smaragdakis Y (2020) MadMax: Analyzing the
out-of-Gas World of Smart Contracts. Commun ACM 63(10):87–95, DOI 10.1145/3416262, URL
https://doi.org/10.1145/3416262
<br>

Grieco G, Song W, Cygan A, Feist J, Groce A (2020) Echidna: Effective, Usable, and Fast Fuzzing for
Smart Contracts. In: Proceedings of the 29th ACM SIGSOFT International Symposium on Soft-
ware Testing and Analysis, Association for Computing Machinery, New York, NY, USA, ISSTA
2020, pp 557–560, DOI 10.1145/3395363.3404366, URL https://doi.org/10.1145/3395363.
3404366
<br>

Grishchenko I, Maffei M, Schneidewind C (2018) A Semantic Framework for the Security Anal-
ysis of Ethereum Smart Contracts. In: Bauer L, Küsters R (eds) Principles of Security and
Trust, vol 10804, Springer International Publishing, Uppsala, Sweden, pp 243–269, DOI 10.1007/
978-3-319-89722-6_10, URL http://link.springer.com/10.1007/978-3-319-89722-6_10
<br>

Hajdu Á, Jovanović D (2020) solc-verify: A Modular Verifier for Solidity Smart Contracts. pp
161–179, DOI 10.1007/978-3-030-41600-3_11
<br>

Hansman S, Hunt R (2005) A taxonomy of network and computer attacks. Computers & Security
24(1):31–43, DOI 10.1016/j.cose.2004.06.011, URL https://www.sciencedirect.com/science/
article/pii/S0167404804001804
<br>

Hartel P, Schumi R (2020) Mutation Testing of Smart Contracts at Scale. In: Lec-
ture Notes in Computer Science (including subseries Lecture Notes in Artificial Intelli-
gence and Lecture Notes in Bioinformatics), vol 12165 LNCS, pp 23–42, DOI 10.1007/
978-3-030-50995-8_2, URL http://arxiv.org/abs/1909.12563http://link.springer.com/
10.1007/978-3-030-50995-8_2, 1909.12563
<br>

Howard JD (1997) An analysis of security incidents on the Internet 1989-1995. PhD
thesis, Carnegie Mellon University, USA, URL https://www.proquest.com/openview/
26b4425b41777ee9b6cac10b78da998a/1?pq-origsite=gscholar&cbl=18750&diss=y
<br>

Hu B, Zhang Z, Liu J, Liu Y, Yin J, Lu R, Lin X (2021) A comprehensive survey on smart
contract construction and execution: paradigms, tools, and systems. Patterns 2(2):100179, DOI
10.1016/j.patter.2020.100179
<br>

Hu T, Li B, Pan Z, Qian C (2023) Detect Defects of Solidity Smart Contract Based on the Knowledge
Graph. IEEE Transactions on Reliability pp 1–17, DOI 10.1109/TR.2023.3233999, URL https:
//ieeexplore.ieee.org/document/10025570/
<br>

IBM (2013a) Orthogonal Defect Classification v 5.2 Extensions for GUI, User Documentation,
Build & NLS. URL https://s3.us.cloud-object-storage.appdomain.cloud/res-files/
70-ODC-5-2-Extensions.pdf
<br>

IBM (2013b) Orthogonal Defect Classification v 5.2 for Software Design and Code. URL https:
//s3.us.cloud-object-storage.appdomain.cloud/res-files/70-ODC-5-2.pdf
<br>

Ji R, He N, Wu L, Wang H, Bai G, Guo Y (2020) DEPOSafe: Demystifying the Fake Deposit Vul-
nerability in Ethereum Smart Contracts. In: 2020 25th International Conference on Engineering
of Complex Computer Systems (ICECCS), IEEE, pp 125–134, DOI 10.1109/ICECCS51672.2020.
00022, URL https://ieeexplore.ieee.org/document/9376204/
<br>

Jiang B, Liu Y, Chan WK (2018) ContractFuzzer: Fuzzing Smart Contracts for Vulnerability Detec-
tion. In: Proceedings of the 33rd ACM/IEEE International Conference on Automated Software
Engineering, Association for Computing Machinery, New York, NY, USA, ASE 2018, pp 259–269,
DOI 10.1145/3238147.3238177, URL https://doi.org/10.1145/3238147.3238177
<br>

Kaleem M, Mavridou A, Laszka A (2020) Vyper: A Security Comparison with Solidity Based on
Common Vulnerabilities. In: 2020 2nd Conference on Blockchain Research & Applications for
Innovative Networks and Services (BRAINS), IEEE, pp 107–111, DOI 10.1109/BRAINS49436.
2020.9223278
<br>

Kalra S, Goel S, Dhawan M, Sharma S (2018) ZEUS: Analyzing Safety of Smart Contracts. In:
Proceedings 2018 Network and Distributed System Security Symposium, Internet Society, Re-
ston, VA, pp 2018–02, DOI 10.14722/ndss.2018.23082, URL https://www.ndss-symposium.
org/wp-content/uploads/2018/02/ndss2018_09-1_Kalra_paper.pdf
<br>

Khan S, Amin MB, Azar AT, Aslam S (2021) Towards Interoperable Blockchains: A Survey on
the Role of Smart Contracts in Blockchain Interoperability. IEEE Access 9:116672–116691, DOI
10.1109/ACCESS.2021.3106384
<br>

Kolluri A, Nikolic I, Sergey I, Hobor A, Saxena P (2019) Exploiting the Laws of Order in Smart
Contracts. In: Proceedings of the 28th ACM SIGSOFT International Symposium on Software
Testing and Analysis, Association for Computing Machinery, New York, NY, USA, ISSTA 2019,
pp 363–373, DOI 10.1145/3293882.3330560, URL https://doi.org/10.1145/3293882.3330560
<br>

Krsul IV (1998) Software vulnerability analysis. PhD thesis, Purdue University, URL https:
//www.proquest.com/openview/10fa0675998eeecf99bbc64ca3a46650/1?pq-origsite=
gscholar&cbl=18750&diss=y
<br>

Krupp J, Rossow C (2018) TEETHER: Gnawing at Ethereum to Automatically Exploit Smart
Contracts. In: Proceedings of the 27th USENIX Conference on Security Symposium, USENIX
Association, USA, SEC’18, pp 1317–1333
<br>

Li X, Xing X, Wang G, Li P, Liu X (2023) Detecting Unknown Vulnerabilities in Smart Con-
tracts with Binary Classification Model Using Machine Learning. pp 179–192, DOI 10.1007/
978-981-99-0272-9_12, URL https://link.springer.com/10.1007/978-981-99-0272-9_12
<br>

Liao JW, Tsai TT, He CK, Tien CW (2019) SoliAudit: Smart Contract Vulnerability Assess-
ment Based on Machine Learning and Fuzz Testing. In: 2019 Sixth International Conference
on Internet of Things: Systems, Management and Security (IOTSMS), IEEE, Granada, Spain,
pp 458–465, DOI 10.1109/IOTSMS48152.2019.8939256, URL https://ieeexplore.ieee.org/
document/8939256/
<br>

Lindqvist U, Jonsson E (1997) How to systematically classify computer security intrusions. pp
154–163
<br>

Liu C, Liu H, Cao Z, Chen Z, Chen B, Roscoe B (2018) ReGuard: Finding Reentrancy Bugs in
Smart Contracts. In: Proceedings of the 40th International Conference on Software Engineering:
Companion Proceeedings, ACM, New York, NY, USA, pp 65–68, DOI 10.1145/3183440.3183495,
URL https://dl.acm.org/doi/10.1145/3183440.3183495
<br>

Lough DL (2001) A taxonomy of computer attacks with applications to wireless networks. PhD
thesis, Virginia Polytechnic Institute and State University
<br>

Lu N, Wang B, Zhang Y, Shi W, Esposito C (2019) NeuCheck: A more practical Ethereum smart
contract security analysis tool. Software: Practice and Experience n/a(n/a):1–20, DOI https://
doi.org/10.1002/spe.2745, URL https://onlinelibrary.wiley.com/doi/abs/10.1002/spe.
2745
<br>

Luu L, Chu DH, Olickel H, Saxena P, Hobor A (2016) Making Smart Contracts Smarter. In:
Proceedings of the 2016 ACM SIGSAC Conference on Computer and Communications Security,
Association for Computing Machinery, New York, NY, USA, CCS ’16, pp 254–269, DOI 10.1145/
2976749.2978309, URL https://doi.org/10.1145/2976749.2978309
<br>

Ma F, Xu Z, Ren M, Yin Z, Chen Y, Qiao L, Gu B, Li H, Jiang Y, Sun J (2022) Pluto: Ex-
posing Vulnerabilities in Inter-Contract Scenarios. IEEE Transactions on Software Engineer-
ing 48(11):4380–4396, DOI 10.1109/TSE.2021.3117966, URL https://ieeexplore.ieee.org/
document/9562567/
<br>

Mann DE, Christey SM (1999) Towards a Common Enumeration of Vulnerabilities. In: 2nd Work-
shop on Research with Security Vulnerability Databases, Purdue University in West Lafayette,
Indiana, pp 1–13
<br>

Manning A (2018) Solidity Security: Comprehensive list of known attack vectors and common
anti-patterns. URL https://github.com/sigp/solidity-security-blog
<br>

Mavridou A, Laszka A, Stachtiari E, Dubey A (2019) VeriSolid: Correct-by-Design Smart Contracts
for Ethereum. In: Goldberg I, Moore T (eds) Financial Cryptography and Data Security, Springer
International Publishing, Cham, pp 446–465
<br>

Mavridou Anastasia, , Laszka A (2018) Designing Secure Ethereum Smart Con-
tracts: A Finite State Machine Based Approach. In: Meiklejohn Sarah, , Sako
K (eds) Financial Cryptography and Data Security, Springer Berlin Heidelberg,
Berlin, Heidelberg, pp 523–540, URL https://www.springerprofessional.de/en/
designing-secure-ethereum-smart-contracts-a-finite-state-machine/17118720
MITRE Corporation (1999) Common Vulnerabilities and Exposures. URL https://www.cve.org/
<br>

Momeni P, Wang Y, Samavi R (2019) Machine Learning Model for Smart Contracts Secu-
rity Analysis. In: 2019 17th International Conference on Privacy, Security and Trust (PST),
IEEE, Fredericton, NB, Canada, pp 1–6, DOI 10.1109/PST47121.2019.8949045, URL https:
//ieeexplore.ieee.org/document/8949045/
<br>

NCC Group (2019) DASP. URL https://dasp.co/
<br>

NCCGroup (2021) Decentralized Application Security Project (DASP) Top10. URL https://dasp.
co/
<br>

Nguyen TD, Pham LH, Sun J, Lin Y, Minh QT (2020) SFuzz: An Efficient Adaptive Fuzzer for
Solidity Smart Contracts. In: Proceedings of the ACM/IEEE 42nd International Conference on
Software Engineering, Association for Computing Machinery, New York, NY, USA, ICSE ’20, pp
778–788, DOI 10.1145/3377811.3380334, URL https://doi.org/10.1145/3377811.3380334
OWASP Foundation (2001) OWASP. URL https://owasp.org/www-community/
vulnerabilities/#
<br>

Qian P, Liu ZG, He QM, Huang BT, Tian DZ, Wang X (2022) Smart Contract Vulnerability
Detection Technique: A Survey. Ruan Jian Xue Bao/Journal of Software 33(8):3059–3085, DOI
10.13328/j.cnki.jos.006375, 2209.05872
<br>

Rameder H, di Angelo M, Salzer G (2022) Review of Automated Vulnerability Analysis of Smart
Contracts on Ethereum. Frontiers in Blockchain 5, DOI 10.3389/fbloc.2022.814977
Rodler M, Li W, Karame GO, Davi L (2019) Sereum: Protecting Existing Smart Contracts
Against Re-Entrancy Attacks. In: Proceedings 2019 Network and Distributed System Security
Symposium, Internet Society, Reston, VA, DOI 10.14722/ndss.2019.23413, URL https://www.
ndss-symposium.org/wp-content/uploads/2019/02/ndss2019_09-3_Rodler_paper.pdf
<br>

Siegel D (2016) Understanding The DAO Attack. URL https://www.coindesk.com/understanding-dao-hack-journalists
<br>

Slither’s Github (2019) Slither Vulnerabilities Detection. URL https://github.com/crytic/slither
<br>

SmartContractSecurity (2020) Smart Contract Weakness Classification (SWC) and Test Cases. URL http://swcregistry.io/
<br>

SmartDec Corporation (2018) SmartDec - Classification of smart contract vulnerabilities. URL https://github.com/smartdec/classification
<br>

So S, Lee M, Park J, Lee H, Oh H (2020) VERISMART: A Highly Precise Safety Verifier
for Ethereum Smart Contracts. In: 2020 IEEE Symposium on Security and Privacy (SP),
IEEE, San Francisco, CA, USA, pp 1678–1694, DOI 10.1109/SP40000.2020.00032, URL https:
//ieeexplore.ieee.org/document/9152689/
<br>

Solidity (2023) Solidity Documentation 0.8.17. URL https://docs.soliditylang.org/en/v0.8.
17/contracts.html
<br>

Song Jingjing and He H, Zhuo L, Chunhua S, Guangquan X, Wei W (2019) An Efficient Vulnerability
Detection Model for Ethereum Smart Contracts. In: Liu Joseph K and Huang X (ed) Network
and System Security, Springer International Publishing, Cham, pp 433–442
Staderini M, Palli C, Bondavalli A (2020) Classification of Ethereum Vulnerabilities and their
Propagations. In: 2020 Second International Conference on Blockchain Computing and Applications (BCCA), IEEE, pp 44–51, DOI 10.1109/BCCA50787.2020.9274458, URL https:
//ieeexplore.ieee.org/document/9274458/
<br>

Staderini M, Pataricza A, Bondavalli A (2022) Security Evaluation and Improvement of Solidity
Smart Contracts. SSRN Electronic Journal DOI 10.2139/ssrn.4038087, URL https://www.ssrn.
com/abstract=4038087
<br>

Stephens J, Ferles K, Mariano B, Lahiri S, Dillig I (2021) SmartPulse: Automated Checking of
Temporal Properties in Smart Contracts. In: 2021 IEEE Symposium on Security and Privacy
(SP), IEEE, pp 555–571, DOI 10.1109/SP40001.2021.00085
<br>

Tikhomirov S, Voskresenskaya E, Ivanitskiy I, Takhaviev R, Marchenko E, Alexandrov Y (2018)
SmartCheck: Static Analysis of Ethereum Smart Contracts. In: Proceedings of the 1st Interna-
tional Workshop on Emerging Trends in Software Engineering for Blockchain, ACM, New York,
NY, USA, pp 9–16, DOI 10.1145/3194113.3194115, URL https://dl.acm.org/doi/10.1145/
3194113.3194115
<br>

Torres CF, Schütte J, State R (2018) Osiris: Hunting for Integer Bugs in Ethereum Smart Contracts.
In: Proceedings of the 34th Annual Computer Security Applications Conference, Association for
Computing Machinery, New York, NY, USA, ACSAC ’18, pp 664–676, DOI 10.1145/3274694.
3274737, URL https://doi.org/10.1145/3274694.3274737
<br>

Tsankov P (2018) Securify2. URL https://github.com/eth-sri/securify2
<br>

Tsankov P, Dan A, Drachsler-Cohen D, Gervais A, Bünzli F, Vechev M (2018) Securify: Practical
Security Analysis of Smart Contracts. In: Proceedings of the 2018 ACM SIGSAC Conference
on Computer and Communications Security, Association for Computing Machinery, New York,
NY, USA, CCS ’18, pp 67–82, DOI 10.1145/3243734.3243780, URL https://doi.org/10.1145/
3243734.3243780
<br>

Vidal F, Ivaki N, Laranjeiro N (2023a) OpenSCV: An Open Hierachical Taxonomy for Smart
Contract Vulnerabilities – Supplemental Material. DOI 10.5281/zenodo.7763982, URL https:
//doi.org/10.5281/zenodo.7763982
<br>

Vidal F, Ivaki N, Laranjeiro N (2023b) OpenSCV Github Repository. URL https://github.com/
blockchain-dei/openscv
<br>

Vidal F, Ivaki N, Laranjeiro N (2023c) OpenSCV Website. URL http://openscv.dei.uc.pt
<br>

Vogelsteller F, Buterin V (2015) ERC20 standard. URL https://github.com/ethereum/eips/
issues/20
<br>

Wagner G (2018) EIP-1470: Smart Contract Weakness Classification (SWC), URL https://
github.com/ethereum/EIPs/issues/1469
<br>

Wang H, Li Y, Lin SW, Ma L, Liu Y (2019) VULTRON: Catching Vulnerable Smart Contracts
Once and for All. In: 2019 IEEE/ACM 41st International Conference on Software Engineering:
New Ideas and Emerging Results (ICSE-NIER), IEEE, Montreal, QC, Canada, pp 1–4, DOI
10.1109/ICSE-NIER.2019.00009, URL https://ieeexplore.ieee.org/document/8805696/
<br>

Wang W, Song J, Xu G, Li Y, Wang H, Su C (2021) ContractWard: Automated Vulnerability De-
tection Models for Ethereum Smart Contracts. IEEE Transactions on Network Science and En-
gineering 8(2):1133–1144, DOI 10.1109/TNSE.2020.2968505, URL https://ieeexplore.ieee.
org/document/8967006/
<br>

Xi R, Pattabiraman K (2023) A large-scale empirical study of low-level function use in Ethereum
smart contracts and automated replacement. Software: Practice and Experience 53(3):631–664,
DOI 10.1002/spe.3163
<br>

Yaga D, Mell P, Roby N, Scarfone K (2018) Blockchain technology overview. Tech. rep., National
Institute of Standards and Technology, Gaithersburg, MD, DOI 10.6028/NIST.IR.8202, URL
https://nvlpubs.nist.gov/nistpubs/ir/2018/NIST.IR.8202.pdf
<br>

Ye J, Ma M, Lin Y, Sui Y, Xue Y (2020) Clairvoyance: Cross-Contract Static Analysis for Detecting
Practical Reentrancy Vulnerabilities in Smart Contracts. In: Proceedings of the ACM/IEEE
42nd International Conference on Software Engineering: Companion Proceedings, Association
for Computing Machinery, New York, NY, USA, ICSE ’20, pp 274–275, DOI 10.1145/3377812.
3390908, URL https://doi.org/10.1145/3377812.3390908
<br>

Yosifova VK, Bontchev VV (2021) Possible Instant Messaging Malware Attack Using Unicode
Right-To-Left Override. pp 179–191, DOI 10.1007/978-3-030-65722-2_11, URL https://link.
springer.com/10.1007/978-3-030-65722-2_11
<br>

Zhang P, Xiao F, Luo X (2019) SolidityCheck : Quickly Detecting Smart Contract Problems
Through Regular Expressions, URL https://arxiv.org/abs/1911.09425
<br>

Zhang Q, Wang Y, Li J, Ma S (2020) EthPloit: From Fuzzing to Efficient Exploit Generation
against Smart Contracts. In: 2020 IEEE 27th International Conference on Software Analysis,
Evolution and Reengineering (SANER), IEEE, London, ON, Canada, pp 116–126, DOI 10.1109/
SANER48275.2020.9054822, URL https://ieeexplore.ieee.org/document/9054822/
<br>

Zheng G, Gao L, Huang L, Guan J (2021) Ethereum Smart Contract Development in Solidity.
Springer Singapore, Singapore, DOI 10.1007/978-981-15-6218-1, URL http://link.springer.
com/10.1007/978-981-15-6218-1
<br>

Zhou H, Milani Fard A, Makanju A (2022) The State of Ethereum Smart Contracts Security: Vulner-
abilities, Countermeasures, and Tool Support. Journal of Cybersecurity and Privacy 2(2):358–378,
DOI 10.3390/jcp2020019
<br>

Zou W, Lo D, Kochhar PS, Le XBD, Xia X, Feng Y, Chen Z, Xu B (2019) Smart Contract
Development: Challenges and Opportunities. IEEE Transactions on Software Engineering p 1,
DOI 10.1109/TSE.2019.2942301











