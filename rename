import os
import sys


def generate_filename_list(folder_path, output_file):
    """
    生成包含所有带占位符文件名的文本文件
    """
    # 确保输出文件以.txt结尾
    if not output_file.lower().endswith('.txt'):
        output_file += '.txt'

    # 获取文件夹中的所有文件
    all_files = os.listdir(folder_path)
    files = [f for f in all_files if os.path.isfile(os.path.join(folder_path, f))]

    if not files:
        print("该文件夹中没有可处理的文件！")
        return False

    print(f"找到 {len(files)} 个文件，正在生成文件名列表...")

    # 创建带占位符的文件名列表
    filename_list = []
    for filename in files:
        # 分离文件名和扩展名
        name_part, ext = os.path.splitext(filename)

        # 创建带占位符的新文件名
        new_filename = f"{name_part}_文件作用城市_文件来源_发表日期{ext}"
        filename_list.append(new_filename)

    # 写入文本文件
    try:
        with open(output_file, 'w', encoding='utf-8') as f:
            for name in filename_list:
                f.write(name + '\n')

        print(f"成功生成文件名列表文件: {os.path.abspath(output_file)}")
        print("请打开此文件编辑占位符部分（城市、来源、日期）")
        return True
    except Exception as e:
        print(f"写入文件失败: {str(e)}")
        return False


def batch_rename_from_list(folder_path, list_file):
    """
    根据修改后的文件名列表进行批量重命名
    """
    # 读取文件名列表
    try:
        with open(list_file, 'r', encoding='utf-8') as f:
            new_names = [line.strip() for line in f.readlines() if line.strip()]
    except Exception as e:
        print(f"读取列表文件失败: {str(e)}")
        return False

    # 获取原始文件名列表
    all_files = os.listdir(folder_path)
    files = [f for f in all_files if os.path.isfile(os.path.join(folder_path, f))]

    if len(new_names) != len(files):
        print(f"警告: 列表文件中有 {len(new_names)} 个文件名，但文件夹中有 {len(files)} 个文件")
        print("请确保两者数量一致后再继续")
        return False

    print("\n将要执行的重命名操作:")
    print("-" * 60)
    for orig, new in zip(files, new_names):
        print(f"{orig}  →  {new}")
    print("-" * 60)

    # 确认操作
    confirm = input("\n确定要执行这些重命名操作吗？(y/n): ").strip().lower()
    if confirm != 'y':
        print("操作已取消。")
        return False

    success_count = 0
    error_count = 0

    # 执行重命名
    for orig_name, new_name in zip(files, new_names):
        orig_path = os.path.join(folder_path, orig_name)
        new_path = os.path.join(folder_path, new_name)

        try:
            # 检查新文件名是否已存在
            if os.path.exists(new_path) and orig_path != new_path:
                print(f"✗ 跳过: {orig_name} → {new_name} (目标文件已存在)")
                error_count += 1
                continue

            os.rename(orig_path, new_path)
            print(f"✓ 已重命名: {orig_name} → {new_name}")
            success_count += 1
        except Exception as e:
            print(f"✗ 重命名失败: {orig_name} → {new_name} - {str(e)}")
            error_count += 1

    print("\n" + "=" * 50)
    print(f"操作完成! 成功: {success_count}, 失败: {error_count}, 总计: {len(files)}")
    print("=" * 50)
    return True


def get_valid_folder_path(prompt):
    """获取有效的文件夹路径"""
    while True:
        path = input(prompt).strip()

        # 处理拖放文件夹到终端的情况
        if path.startswith(('"', "'")) and path.endswith(('"', "'")):
            path = path[1:-1]

        # 检查路径是否存在
        if not os.path.exists(path):
            print(f"错误：路径 '{path}' 不存在！")
            continue

        # 检查是否为文件夹
        if not os.path.isdir(path):
            print(f"错误：'{path}' 不是文件夹！")
            continue

        return os.path.abspath(path)


def get_valid_file_path(prompt, extension=".txt"):
    """获取有效的文件路径"""
    while True:
        path = input(prompt).strip()

        # 处理拖放文件到终端的情况
        if path.startswith(('"', "'")) and path.endswith(('"', "'")):
            path = path[1:-1]

        # 添加默认扩展名
        if not os.path.splitext(path)[1] and extension:
            path += extension

        # 检查文件是否存在
        if not os.path.exists(path):
            create = input(f"文件 '{path}' 不存在，是否创建?(y/n): ").strip().lower()
            if create == 'y':
                return path
            continue

        # 检查是否为文件
        if not os.path.isfile(path):
            print(f"错误：'{path}' 不是文件！")
            continue

        return path


def print_menu():
    """打印主菜单"""
    print("\n" + "=" * 60)
    print("文件批量重命名工具".center(60))
    print("=" * 60)
    print("1. 生成文件名列表文件（带占位符）")
    print("2. 使用文件名列表进行批量重命名")
    print("3. 退出")
    print("=" * 60)


def print_welcome():
    """打印欢迎信息"""
    print("\n" + "=" * 70)
    print("高级文件批量重命名系统".center(70))
    print("=" * 70)
    print("功能说明：")
    print("1. 两阶段操作：首先生成带占位符的文件名列表，编辑后再执行重命名")
    print("2. 格式: 原始文件名 → 新文件名_城市_来源_日期.扩展名")
    print("3. 支持绝对路径和相对路径")
    print("4. 提供操作确认和安全检查")
    print("=" * 70)
    print(f"当前工作目录: {os.getcwd()}")
    print("=" * 70)


def main():
    """主函数"""
    print_welcome()

    while True:
        print_menu()
        choice = input("请选择操作 (1-3): ").strip()

        if choice == '1':
            # 生成文件名列表
            print("\n[生成文件名列表]")
            folder = get_valid_folder_path("请输入源文件夹路径: ")
            output_file = input("请输入输出文件名 (默认: filenames.txt): ").strip() or "filenames.txt"
            generate_filename_list(folder, output_file)

        elif choice == '2':
            # 执行批量重命名
            print("\n[执行批量重命名]")
            folder = get_valid_folder_path("请输入源文件夹路径: ")
            list_file = get_valid_file_path("请输入文件名列表文件路径: ", ".txt")
            batch_rename_from_list(folder, list_file)

        elif choice == '3':
            print("感谢使用，再见！")
            break

        else:
            print("无效选择，请重新输入！")

        input("\n按Enter键继续...")


if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\n操作被用户中断。")
    except Exception as e:
        print(f"\n发生错误: {str(e)}")

    # 在Windows系统下保持窗口打开
    if sys.platform.startswith('win'):
        input("\n按Enter键退出...")
