<script>
    import { Client, Account } from 'appwrite';
    import { onMount } from 'svelte';
  
    // --- Appwrite Setup ---
    let appwriteEndpoint = 'https://fra.cloud.appwrite.io/v1'; // TODO: Replace with your endpoint
    let appwriteProject = '6852d529001fe30042c0'; // TODO: Replace with your project ID
  
    const client = new Client();
    client.setEndpoint(appwriteEndpoint).setProject(appwriteProject);
    const account = new Account(client);
  
    // --- Auth State ---
    let user = null;
    let email = '';
    let password = '';
    let authError = '';
    let isLogin = true;
    let showPassword = false;
    let signupSuccess = false;
  
    // --- Expense State ---
    let expenses = [];
    let expenseDesc = '';
    let expenseAmount = '';
  
    // --- Auth Functions ---
    async function login() {
      authError = '';
      try {
        await account.createEmailPasswordSession(email, password);
        await fetchUser();
      } catch (e) {
        if (e.code === 401) {
          authError = 'Invalid credentials. Please check the email and password.';
        } else {
          authError = e.message;
        }
      }
    }
  
    async function signup() {
      authError = '';
      signupSuccess = false;
      try {
        await account.create('unique()', email, password);
        signupSuccess = true;
      } catch (e) {
        if (e.code === 409) {
          authError = 'User already exists. Please log in.';
        } else if (e.code === 400 && password.length < 8) {
          authError = 'Password must be at least 8 characters.';
        } else {
          authError = e.message;
        }
      }
    }
  
    async function logout() {
      await account.deleteSession('current');
      user = null;
    }
  
    async function fetchUser() {
      try {
        user = await account.get();
      } catch {
        user = null;
      }
    }
  
    onMount(fetchUser);
  
    // --- Expense Functions (local only) ---
    function addExpense() {
      if (!expenseDesc || !expenseAmount) return;
      expenses = [
        ...expenses,
        { desc: expenseDesc, amount: parseFloat(expenseAmount), id: Date.now() }
      ];
      expenseDesc = '';
      expenseAmount = '';
    }
  
    function deleteExpense(id) {
      expenses = expenses.filter(e => e.id !== id);
    }
  </script>
  
  <style>
    * {
      box-sizing: border-box;
    }
  
    :global(body) {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      padding: 1rem;
    }
  
    .container {
      max-width: 480px;
      margin: 0 auto;
      padding: 2rem;
    }
  
    .card {
      background: rgba(255, 255, 255, 0.95);
      backdrop-filter: blur(10px);
      border-radius: 20px;
      padding: 2rem;
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
      border: 1px solid rgba(255, 255, 255, 0.2);
      margin-bottom: 1.5rem;
    }
  
    .auth-header {
      text-align: center;
      margin-bottom: 2rem;
    }
  
    .auth-header h2 {
      color: #333;
      font-size: 2rem;
      margin: 0;
      font-weight: 600;
    }
  
    .auth-header .subtitle {
      color: #666;
      margin-top: 0.5rem;
      font-size: 0.9rem;
    }
  
    .form-group {
      margin-bottom: 1.5rem;
    }
  
    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
      color: #333;
      font-weight: 500;
      font-size: 0.9rem;
    }
  
    input {
      width: 100%;
      padding: 0.75rem 1rem;
      border: 2px solid #e1e5e9;
      border-radius: 12px;
      font-size: 1rem;
      transition: all 0.3s ease;
      background: #fff;
    }
  
    input:focus {
      outline: none;
      border-color: #667eea;
      box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
    }
  
    .password-container {
      position: relative;
    }
  
    .password-toggle {
      position: absolute;
      right: 0.75rem;
      top: 50%;
      transform: translateY(-50%);
      background: none;
      border: none;
      color: #666;
      cursor: pointer;
      font-size: 0.8rem;
      padding: 0.25rem;
      width: auto;
      margin: 0;
    }
  
    .password-toggle:hover {
      color: #333;
    }
  
    .btn {
      width: 100%;
      padding: 0.875rem;
      border: none;
      border-radius: 12px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      margin-bottom: 0.75rem;
    }
  
    .btn-primary {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
    }
  
    .btn-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 25px rgba(102, 126, 234, 0.4);
    }
  
    .btn-secondary {
      background: #f8f9fa;
      color: #6c757d;
      border: 2px solid #e9ecef;
    }
  
    .btn-secondary:hover {
      background: #e9ecef;
      color: #495057;
    }
  
    .btn-danger {
      background: #dc3545;
      color: white;
      width: auto;
      padding: 0.5rem 1rem;
      font-size: 0.8rem;
      margin: 0;
    }
  
    .btn-danger:hover {
      background: #c82333;
      transform: translateY(-1px);
    }
  
    .btn-success {
      background: linear-gradient(135deg, #28a745 0%, #20c997 100%);
      color: white;
    }
  
    .btn-success:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 25px rgba(40, 167, 69, 0.4);
    }
  
    .alert {
      padding: 1rem;
      border-radius: 12px;
      margin-bottom: 1rem;
      font-size: 0.9rem;
    }
  
    .alert-error {
      background: #f8d7da;
      color: #721c24;
      border: 1px solid #f5c6cb;
    }
  
    .alert-success {
      background: #d4edda;
      color: #155724;
      border: 1px solid #c3e6cb;
    }
  
    .welcome-header {
      text-align: center;
      margin-bottom: 1.5rem;
    }
  
    .welcome-header h2 {
      color: #333;
      margin: 0;
      font-size: 1.5rem;
    }
  
    .welcome-header .user-email {
      color: #667eea;
      font-weight: 500;
    }
  
    .expense-header {
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 2rem;
    }
  
    .expense-header h3 {
      color: #333;
      margin: 0;
      font-size: 1.8rem;
      font-weight: 600;
    }
  
    .expense-header .icon {
      font-size: 2rem;
      margin-right: 0.5rem;
    }
  
    .expense-form {
      display: grid;
      gap: 1rem;
      margin-bottom: 2rem;
    }
  
    .form-row {
      display: grid;
      grid-template-columns: 2fr 1fr;
      gap: 1rem;
    }
  
    .expense-list {
      list-style: none;
      padding: 0;
      margin: 0;
    }
  
    .expense-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem;
      background: #f8f9fa;
      border-radius: 12px;
      margin-bottom: 0.75rem;
      transition: all 0.3s ease;
    }
  
    .expense-item:hover {
      background: #e9ecef;
      transform: translateX(4px);
    }
  
    .expense-details {
      flex: 1;
    }
  
    .expense-desc {
      font-weight: 500;
      color: #333;
      margin-bottom: 0.25rem;
    }
  
    .expense-amount {
      font-size: 1.1rem;
      font-weight: 600;
      color: #667eea;
    }
  
    .total-section {
      background: linear-gradient(135deg, #28a745 0%, #20c997 100%);
      color: white;
      padding: 1.5rem;
      border-radius: 16px;
      text-align: center;
      margin-top: 2rem;
    }
  
    .total-label {
      font-size: 1rem;
      opacity: 0.9;
      margin-bottom: 0.5rem;
    }
  
    .total-amount {
      font-size: 2rem;
      font-weight: 700;
      margin: 0;
    }
  
    .empty-state {
      text-align: center;
      padding: 3rem 1rem;
      color: #6c757d;
    }
  
    .empty-state .icon {
      font-size: 3rem;
      margin-bottom: 1rem;
      opacity: 0.5;
    }
  
    @media (max-width: 600px) {
      .container {
        padding: 1rem;
      }
      
      .card {
        padding: 1.5rem;
      }
      
      .form-row {
        grid-template-columns: 1fr;
      }
      
      .expense-item {
        flex-direction: column;
        align-items: flex-start;
        gap: 1rem;
      }
    }
  </style>
  
  <div class="container">
    {#if !user}
      <div class="card">
        <div class="auth-header">
          <h2>💰 Expense Tracker</h2>
          <div class="subtitle">{isLogin ? 'Welcome back!' : 'Create your account'}</div>
        </div>
        
        <div class="form-group">
          <label for="endpoint">Appwrite Endpoint</label>
          <input id="endpoint" placeholder="https://fra.cloud.appwrite.io/v1" bind:value={appwriteEndpoint} />
        </div>
        
        <div class="form-group">
          <label for="project">Project ID</label>
          <input id="project" placeholder="Your project ID" bind:value={appwriteProject} />
        </div>
        
        <div class="form-group">
          <label for="email">Email Address</label>
          <input id="email" type="email" placeholder="your@email.com" bind:value={email} />
        </div>
        
        <div class="form-group">
          <label for="password">Password</label>
          <div class="password-container">
            {#if showPassword}
              <input
                id="password"
                type="text"
                placeholder="Enter your password"
                bind:value={password}
              />
            {:else}
              <input
                id="password"
                type="password"
                placeholder="Enter your password"
                bind:value={password}
              />
            {/if}
            <button 
              type="button" 
              class="password-toggle" 
              on:click={() => showPassword = !showPassword}
            >
              {showPassword ? '👁️' : '👁️‍🗨️'}
            </button>
          </div>
        </div>
        
        {#if authError}
          <div class="alert alert-error">{authError}</div>
        {/if}
        
        {#if signupSuccess}
          <div class="alert alert-success">
            ✅ Signup successful! Please verify your email if required, then log in.
          </div>
        {/if}
        
        <button class="btn btn-primary" on:click={isLogin ? login : signup}>
          {isLogin ? '🔐 Login' : '🚀 Sign Up'}
        </button>
        
        <button 
          class="btn btn-secondary" 
          on:click={() => { isLogin = !isLogin; authError = ''; signupSuccess = false; }}
        >
          {isLogin ? 'Need an account? Sign Up' : 'Have an account? Login'}
        </button>
      </div>
    {:else}
      <div class="card">
        <div class="welcome-header">
          <h2>Welcome back, <span class="user-email">{user.email}</span></h2>
        </div>
        <button class="btn btn-secondary" on:click={logout}>🚪 Logout</button>
      </div>
      
      <div class="card">
        <div class="expense-header">
          <span class="icon">📊</span>
          <h3>Expense Tracker</h3>
        </div>
        
        <div class="expense-form">
          <div class="form-row">
            <div class="form-group">
              <label for="desc">Description</label>
              <input id="desc" placeholder="What did you spend on?" bind:value={expenseDesc} />
            </div>
            <div class="form-group">
              <label for="amount">Amount ($)</label>
              <input id="amount" type="number" step="0.01" placeholder="0.00" bind:value={expenseAmount} />
            </div>
          </div>
          <button class="btn btn-success" on:click={addExpense}>
            ➕ Add Expense
          </button>
        </div>
        
        {#if expenses.length === 0}
          <div class="empty-state">
            <div class="icon">📝</div>
            <p>No expenses yet. Add your first expense above!</p>
          </div>
        {:else}
          <ul class="expense-list">
            {#each expenses as exp (exp.id)}
              <li class="expense-item">
                <div class="expense-details">
                  <div class="expense-desc">{exp.desc}</div>
                  <div class="expense-amount">${exp.amount.toFixed(2)}</div>
                </div>
                <button class="btn btn-danger" on:click={() => deleteExpense(exp.id)}>
                  🗑️
                </button>
              </li>
            {/each}
          </ul>
          
          <div class="total-section">
            <div class="total-label">Total Expenses</div>
            <div class="total-amount">${expenses.reduce((sum, e) => sum + e.amount, 0).toFixed(2)}</div>
          </div>
        {/if}
      </div>
    {/if}
  </div>