# Git�ʼ�

## ���
Git��Ŀǰ���������Ƚ��ķֲ�ʽ�汾����ϵͳ

## ��������

1. �����û���Ϣ(Username��Email) 
> git config --global user.name "Your Name"
> git config --global user.email "email@example.com"

2. ����Git�ֿ�

> git init

```
����Ŀ��Ŀ¼
�л�������		�̷�:
�л�Ŀ¼		cd Ŀ¼��
������һ��Ŀ¼		cd ..
������ʷ��������	�������°���
```

3. �鿴�ֿ�״̬
> git status

4. ����ļ�(��Git������)
> git add �ļ���/--all
> git commit -m "��־��¼������"

5. �Ա��ļ���ͬ
> git diff �ļ���

6. �鿴��ʷ��¼[������ʾ]
> git log [--pretty=oneline]
> git reflog

7. �汾����
> git reset --hard HEAD^(HEAD^^/HEAD~5)
> git reset --hard commit id(commitֻ��дһС���ּ���)

8. ���ع�����
> git checkout -- �ļ���

9. �����ݴ���
> git reset HEAD �ļ���

10. ɾ���汾���ļ�(ɾ������Ҫcommit)
> git rm �ļ���

11. ��֧����
> git branch ��֧��

12. �л���֧
> git checkout ��֧��

13. ���������л���֧
> git checkout -b ��֧��

14. ɾ����֧
> git branch -d ��֧��

15. �ϲ���֧(������Ŀ���֧�ϰѷ�֧�ϲ�����)
> git merge ���ϲ��ķ�֧
