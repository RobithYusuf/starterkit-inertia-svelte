<script>
    import { useForm } from '@inertiajs/svelte';
    import { Link } from '@inertiajs/svelte';
    import AuthLayout from '@/Components/Layouts/AuthLayout.svelte';
    import Button from '@/Components/UI/Button.svelte';
    import PasswordInput from '@/Components/UI/Form/PasswordInput.svelte';
    import EmailInput from '@/Components/UI/Form/EmailInput.svelte';
    import FormError from '@/Components/UI/Form/FormError.svelte';
    
    const form = useForm({
        email: '',
        password: '',
        remember: false,
    });
    
    function submit() {
        $form.post('/login', {
            onError: (errors) => {
                console.error('Login errors:', errors);
            },
        });
    }
</script>

<AuthLayout 
    title="Welcome back"
    subtitle="Sign in to your account to continue"
>
    <!-- Error Alert -->
    {#if Object.keys($form.errors).length > 0}
        <div class="mb-6">
            <FormError errors={$form.errors} title="Login failed" />
        </div>
    {/if}
    
    <!-- Form -->
    <form onsubmit={(e) => { e.preventDefault(); submit(); }} class="space-y-5">
        <!-- Email Field -->
        <div>
            <label for="email" class="block text-sm font-semibold text-gray-700 mb-2">
                Email Address
            </label>
            <EmailInput
                id="email"
                bind:value={$form.email}
                error={$form.errors.email}
                placeholder="name@company.com"
                required
            />
        </div>
        
        <!-- Password Field -->
        <div>
            <div class="flex items-center justify-between mb-2">
                <label for="password" class="block text-sm font-semibold text-gray-700">
                    Password
                </label>
                <Link href="/forgot-password" class="text-sm font-medium text-primary-600 hover:text-primary-700 transition-colors">
                    Forgot password?
                </Link>
            </div>
            <PasswordInput
                id="password"
                bind:value={$form.password}
                placeholder="••••••••"
                error={$form.errors.password}
                required
            />
        </div>
        
        <!-- Remember Me -->
        <div class="flex items-center">
            <label class="relative flex items-center cursor-pointer group">
                <input
                    type="checkbox"
                    bind:checked={$form.remember}
                    class="w-4 h-4 border-gray-300 rounded transition-colors"
                    style="accent-color: var(--theme-primary-600)"
                />
                <span class="ml-2 text-sm text-gray-600 group-hover:text-gray-900 transition-colors">
                    Keep me signed in
                </span>
            </label>
        </div>
        
        <!-- Submit Button -->
        <div class="pt-2">
            <Button
                type="submit"
                variant="primary"
                size="lg"
                class="w-full"
                loading={$form.processing}
                disabled={$form.processing}
            >
                {#if $form.processing}
                    Signing in...
                {:else}
                    Sign in
                {/if}
            </Button>
        </div>
    </form>
    
    <!-- Divider -->
    <div class="relative my-8">
        <div class="absolute inset-0 flex items-center">
            <div class="w-full border-t border-gray-200"></div>
        </div>
        <div class="relative flex justify-center text-sm">
            <span class="px-4 bg-white text-gray-500">New to our platform?</span>
        </div>
    </div>
    
    <!-- Register Link -->
    <Link 
        href="/register" 
        class="w-full flex items-center justify-center gap-2 px-4 py-3 border-2 border-gray-200 rounded-xl text-gray-700 font-medium hover:border-primary-300 hover:text-primary-600 hover:bg-primary-50/50 transition-all duration-200"
    >
        Create an account
    </Link>
</AuthLayout>
