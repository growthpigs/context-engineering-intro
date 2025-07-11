# Programming Rules

## 🚨 CRITICAL: File Naming & Import Rules (PREVENT BROKEN LINKS)

### File Naming Conventions - STRICT RULES
```
✅ ALWAYS USE:
- lowercase-kebab-case for ALL files: dashboard-view.tsx, user-service.ts
- Lowercase extensions: .tsx, .ts, .css (never .TSX or .TS)
- Hyphens for spaces: user-profile.tsx (NOT userProfile.tsx or user_profile.tsx)
- Descriptive names: user-dashboard.tsx (NOT dashboard.tsx)

❌ NEVER USE:
- PascalCase for files: DashboardView.tsx ❌
- camelCase for files: dashboardView.tsx ❌
- Spaces in filenames: user profile.tsx ❌
- Generic names: index.tsx, styles.css ❌ (except for barrel exports)
```

### Import Rules - PREVENT BROKEN LINKS
```typescript
// ✅ CORRECT: Always use explicit extensions for relative imports
import { UserProfile } from './components/user-profile.tsx';
import { formatDate } from '../utils/format-date.ts';
import type { User } from '../types/user.types.ts';

// ❌ WRONG: Missing extensions cause breaks
import { UserProfile } from './components/user-profile';  // ❌ BREAKS!

// ✅ CORRECT: Use path aliases for deep imports
import { Button } from '@/components/ui/button';
import { useAuth } from '@/hooks/use-auth';

// ❌ WRONG: Deep relative paths are fragile
import { Button } from '../../../components/ui/button'; // ❌ FRAGILE!
```

### Component Export/Import Pattern
```typescript
// ✅ CORRECT: Named exports with consistent naming
// File: components/user-profile.tsx
export const UserProfile: React.FC<UserProfileProps> = () => { ... };

// Import:
import { UserProfile } from '@/components/user-profile.tsx';

// ❌ WRONG: Default exports lead to naming inconsistencies
export default UserProfile; // ❌ Can be imported with any name!
```

### Path Alias Configuration (tsconfig.json)
```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"],
      "@/components/*": ["./src/components/*"],
      "@/hooks/*": ["./src/hooks/*"],
      "@/utils/*": ["./src/utils/*"],
      "@/services/*": ["./src/services/*"],
      "@/types/*": ["./src/types/*"]
    }
  }
}
```

### File Organization Rules
```
src/
├── components/
│   ├── ui/               # shadcn/ui components (don't modify)
│   ├── common/           # Reusable components
│   │   ├── button-group.tsx
│   │   └── loading-spinner.tsx
│   ├── features/         # Feature-specific components
│   │   ├── dashboard/
│   │   │   ├── dashboard-header.tsx
│   │   │   ├── dashboard-sidebar.tsx
│   │   │   └── dashboard-metrics.tsx
│   │   └── auth/
│   │       ├── login-form.tsx
│   │       └── signup-form.tsx
│   └── layouts/
│       ├── app-layout.tsx
│       └── auth-layout.tsx
```

### Import Verification Checklist
Before ANY commit, AI agents must:
1. ✅ Verify all imports have correct file extensions
2. ✅ Check all file paths are lowercase-kebab-case
3. ✅ Ensure no broken imports by checking file existence
4. ✅ Use path aliases for all non-relative imports
5. ✅ Verify case sensitivity (especially for deployments)

### Common Import Errors & Solutions
```typescript
// ERROR: Cannot find module './Dashboard'
// CAUSE: File is actually 'dashboard.tsx' (lowercase)
// FIX: Match exact case
import { Dashboard } from './dashboard.tsx'; // ✅

// ERROR: Module not found './components/UserProfile'
// CAUSE: File renamed but imports not updated
// FIX: Always update ALL imports when renaming
// Use: Find & Replace in entire project

// ERROR: Cannot resolve '@/components/Button'
// CAUSE: Path alias not configured
// FIX: Ensure tsconfig.json has proper paths configuration
```

### AI Agent Instructions for File Operations
```
When creating or modifying files, ALWAYS:
1. Use lowercase-kebab-case for ALL new files
2. Include file extensions in ALL imports
3. Use path aliases (@/) for non-relative imports
4. Run a "find all references" before renaming any file
5. Update ALL imports when moving/renaming files
6. Verify imports work before committing
```

### Git Pre-commit Hook (add to project)
```bash
#!/bin/sh
# Check for PascalCase or camelCase files in src/
if find src -name "*[A-Z]*" -type f | grep -E '\.(tsx?|jsx?|css)$'; then
  echo "❌ Error: Found files with uppercase letters!"
  echo "Please rename all files to lowercase-kebab-case"
  exit 1
fi
```

## Code Standards

### General Principles
- **Type Safety**: Always use TypeScript with strict mode
- **Documentation**: Document all functions, components, and complex logic
- **Error Handling**: Comprehensive error handling with user-friendly messages
- **Testing**: Write tests alongside implementation
- **Performance**: Consider performance implications of all code

### React/TypeScript Standards
```typescript
// Component Structure
interface ComponentNameProps {
  // Props with JSDoc comments
  /** The user's display name */
  name: string;
  /** Optional click handler */
  onClick?: () => void;
}

export const ComponentName: React.FC<ComponentNameProps> = ({ 
  name,
  onClick 
}) => {
  // Hooks at the top
  const [state, setState] = useState<string>('');
  
  // Event handlers
  const handleClick = useCallback(() => {
    onClick?.();
  }, [onClick]);
  
  // Render
  return (
    <div className="component-wrapper">
      {/* JSX content */}
    </div>
  );
};
```

### State Management
- Keep state close to where it's used
- Lift state only when necessary
- Use proper TypeScript types for all state
- Implement optimistic updates for better UX
- Consider using Zustand or Redux Toolkit for complex state

### API Integration
```typescript
// Service pattern
export const userService = {
  async getUser(id: string): Promise<User> {
    try {
      const response = await fetch(`/api/users/${id}`);
      if (!response.ok) throw new Error('User not found');
      return await response.json();
    } catch (error) {
      console.error('Error fetching user:', error);
      throw error;
    }
  }
};

// Hook pattern
export const useUser = (id: string) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    userService.getUser(id)
      .then(setUser)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [id]);

  return { user, loading, error };
};
```

### Testing Standards
- Write tests for all business logic
- Use React Testing Library for component tests
- Aim for 80% code coverage minimum
- Test edge cases and error states
- Use meaningful test descriptions

### Git Workflow
```bash
# Branch naming
feature/description-of-feature
bugfix/description-of-bug
hotfix/description-of-fix

# Commit messages
feat: Add new feature
fix: Fix bug description
docs: Update documentation
style: Format code
refactor: Refactor code
test: Add tests
chore: Update dependencies
```

### Code Review Checklist
- [ ] All files use lowercase-kebab-case naming
- [ ] All imports include file extensions
- [ ] Path aliases used for deep imports
- [ ] TypeScript types are comprehensive
- [ ] Error handling is implemented
- [ ] Tests are written and passing
- [ ] Documentation is updated
- [ ] No console.logs in production code
- [ ] Performance optimizations applied
- [ ] Accessibility standards met